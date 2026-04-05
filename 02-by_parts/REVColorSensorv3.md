# REV Color Sensor V3

Detects **color** (RGB + ambient light) and **proximity** (1–10 cm) over a single I2C connection.

![REV Color Sensor V3 Overview](https://docs.revrobotics.com/~gitbook/image?url=https%3A%2F%2F1166281274-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-ME3KPEhFI6-MDoP9nZD%252Fuploads%252FJm8J0GMHObOJC11WA6mo%252FColor-Sensor-Overview.png%3Falt%3Dmedia%26token%3D1e71fc4d-e106-4a40-a941-468402608ed6&width=768&dpr=3&quality=100&sign=9cef0c96&sv=2)

The REV Color Sensor V3 is built around the **APDS-9151** chip and measures five channels — Red, Green, Blue, Alpha (clear/ambient), and Proximity — through a single sensor. 

The sensor includes a **built-in, software-switchable white LED** for active illumination, and its **IR proximity emitter** can detect objects at a range of roughly **1–10 cm**, making it useful for both color identification and short-range distance detection. Note that color data is best collected **within 2 cm of the target** for the strongest color differentiation.

![REV Control Hub I2C Port Pinout](https://docs.revrobotics.com/~gitbook/image?url=https%3A%2F%2F1359443677-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-M4_pJHI8HTuZFQTNfcy%252F-MFpxo5ekLNYiSEamSF2%252F-MFq-WJhA4JId_hdXUnn%252FControl_Hub_Port_Specific_Pinout_I2C_LARGER_SIZE-01.png%3Falt%3Dmedia%26token%3D12ad81c9-7ad8-4b0f-9ba0-7a2689c15952&width=768&dpr=3&quality=100&sign=775cea4b&sv=2)

It connects to any of the four **I2C ports (Bus 0–3)** on the REV Control Hub using the included 30 cm JST PH 4-pin cable, and communicates at the **I2C address `0x52`**. It operates at **3.3 V**. 

It supports two I2C clock speeds: **Standard mode at 100 kHz** and High Speed mode at 400 kHz. The FTC SDK configures the bus at 100 kHz by default, which means each I2C transaction takes about **10 µs per bit** — reading all five color/proximity registers across multiple sequential byte transfers can take **several milliseconds per full read cycle**. 

As a result, you should avoid calling `sensor.getNormalizedColors()` or similar SDK methods on every loop iteration without accounting for this delay. 

* Call `sensor.getNormalizedColors()` **once per loop** and store the result in a variable — don't call it multiple times per iteration (this is the spirit of the bulk read advice, applied manually).
* The FTC SDK handles I2C transactions on a background thread, so reads are non-blocking, but the data is only as fresh as the last completed transaction (~a few ms at 100 kHz).
* If you need faster/fresher reads, switch the I2C bus to **400 kHz (Fast mode)** via the sensor's configuration — this is a bigger win than any caching strategy.

For practical FTC coding examples — including how to read color components (red, green, blue) and light level in both Blocks and OnBot Java — see the [REV Color Sensor V3 Application Examples](https://docs.revrobotics.com/rev-crossover-products/sensors/color-sensor/application-examples).

---

## Color Spaces — Why Hue Is Better Than Raw RGB

The FTC SDK provides three color spaces. A full overview is at [FTC Color Spaces](https://ftc-docs.firstinspires.org/en/latest/color_processing/color-spaces/color-spaces.html).

| Color Space | Components | Good for |
|---|---|---|
| **RGB** | Red, Green, Blue (0–255 each) | Simple reads; easy to understand |
| **HSV** | Hue, Saturation, Value (0–255 each in SDK) | Robust color identification |
| **YCrCb** | Luminance + red/blue chrominance | Video/camera processing |

### Why not use raw RGB to detect a color?

Raw RGB values change dramatically with lighting conditions. A red game element in shadow reads very differently from the same element under bright arena lights — both the red, green, and blue values shift together. Writing a threshold like `red > 200 && green < 100` will work on your desk but fail at competition.

### Why Hue is more robust

**Hue** is the "pure color" angle around the color wheel, expressed as 0–255 in the FTC SDK (equivalent to 0°–360°). Because Hue is computed from the *ratio* of R, G, B rather than their absolute values, it stays relatively stable as brightness changes. **Saturation** (how vivid vs. washed-out) and **Value** (brightness) absorb the lighting variation, leaving Hue largely unaffected.

Approximate hue ranges for common FTC game element colors. The 0°–360° column matches standard color wheel references; the 0–255 column is the scale used by the FTC SDK's `hsvValues[0]` output from `Color.colorToHSV()`.

**Pure / high-saturation colors:**

| Color | Hue range (0–255) | Hue range (0°–360°) | Notes |
|---|---|---|---|
| Red | 251–255 or 0–7 | 355°–360° or 0°–10° | Wraps around the 0°/360° boundary |
| Yellow | 36–43 | 51°–60° | Centered around 60° |
| Blue | 157–170 | 221°–240° | Centered around 240° |

**Broader "tone" ranges** (include shades and adjacent colors):

| Color tone | Hue range (0–255) | Hue range (0°–360°) |
|---|---|---|
| Red tones | 234–255 or 0–21 | 330°–360° or 0°–30° |
| Yellow tones (incl. orange) | 21–64 | 30°–90° |
| Cyan-blue tones | 143–170 | 201°–240° |
| Green tones | 64–150 | 91°–212° |

### Converting RGB to HSV in code and detect blue color

```java
import com.qualcomm.robotcore.hardware.ColorSensor;
import com.qualcomm.robotcore.hardware.NormalizedColorSensor;
import com.qualcomm.robotcore.hardware.NormalizedRGBA;

...

public void runOpMode() {

    // Retrieve the color sensor from the hardware map using its configuration name
    ColorSensor colorSensor = hardwareMap.get(NormalizedColorSensor.class, "sensor_color");

    // hsvValues is an array that will hold the hue, saturation, and value information.
    float hsvValues[] = {0F,0F,0F};

    // Enable the built-in white LED if the sensor supports it
    if (colorSensor instanceof SwitchableLight) {
        ((SwitchableLight)colorSensor).enableLight(true);
    }

    while (opModeIsActive()) {

        // Read all color channels once per loop — avoid calling getNormalizedColors() multiple times
        // Values are normalized to 0.0–1.0, independent of sensor brand or gain setting
        NormalizedRGBA colors = colorSensor.getNormalizedColors();

        // Convert the RGBA color to HSV — results are written into hsvValues[] (void return)
        Color.colorToHSV(colors.toColor(), hsvValues);

        float hue        = hsvValues[0];  // 0-255
        float saturation = hsvValues[1];  // 0.0 – 1.0  (0 = gray/white, 1 = fully saturated)
        float value      = hsvValues[2];  // 0.0 – 1.0  (0 = black, 1 = full brightness)

        // Display raw RGB and HSV readings on the Driver Station for tuning
        telemetry.addLine()
                .addData("Red", "%.3f", colors.red)
                .addData("Green", "%.3f", colors.green)
                .addData("Blue", "%.3f", colors.blue);
        telemetry.addLine()
                .addData("Hue", "%.3f", hue)
                .addData("Saturation", "%.3f", saturation)
                .addData("Value", "%.3f", value);
        telemetry.addData("Alpha", "%.3f", colors.alpha);

        // Detect blue: hue 221°–240° maps to roughly 157–170 on the 0–255 SDK scale
        // Also require saturation > 0.3 to reject gray/white surfaces
        if (hue > 157 && hue < 170 && saturation > 0.3) {
            // Detected blue
        }
        telemetry.update();
    }
}
```

> **Tip:** Always check **Saturation** alongside Hue. A very low saturation means the sensor is seeing near-white or gray, where the hue reading becomes unreliable. A threshold like `saturation > 0.3` filters out these false reads.

### Detect an object in proximity using the same color sensor

```java
import com.qualcomm.robotcore.hardware.DistanceSensor;

...

// The REV Color Sensor V3 also implements DistanceSensor via its IR proximity emitter
// Useful range is roughly 1–10 cm; accuracy degrades with ambient light and surface color
if (colorSensor instanceof DistanceSensor) {
    telemetry.addData("Distance (cm)", "%.3f", ((DistanceSensor) colorSensor).getDistance(DistanceUnit.CM));
}

// If the object is within 2.5 cm, it is close enough for reliable color detection
if (((DistanceSensor) colorSensor).getDistance(DistanceUnit.CM) <= 2.5) {
    // Object detected — read hue/saturation here for best color accuracy
}

telemetry.update();

```