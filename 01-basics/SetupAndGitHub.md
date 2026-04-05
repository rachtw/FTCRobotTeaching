# Setup & GitHub Guide

This document is a concise step-by-step note. The complete tutorial can be found at [Android Studio Programming Tutorial](https://ftc-docs.firstinspires.org/programming_resources/android_studio_java/Android-Studio-Tutorial.html).

---

## Prerequisites

To run the FTC code, install the following:

1. GitHub Desktop (Git Bash)
2. Android Studio
3. REV Hardware Client

Then log in to the GitHub website.

### Fixing "Android SDK Not Found"

1. Follow [Set up the Android 14 SDK](https://developer.android.com/about/versions/14/setup-sdk)
2. If there is still a problem when compiling (**Build -> Make Project**), choose the first SDK from the prompt, then choose **Try Again** from the second prompt.

---

## Create a Team GitGub Repository (by Team Account Holder)

> **Warning:** Make sure your GitHub folder is **not** stored in a Microsoft OneDrive folder — we need to edit it offline.

### Fork the FTC FtcRobotController Repository (GitHub Steps)

1. Go to FTC's GitHub website:
   [https://github.com/FIRST-Tech-Challenge/FtcRobotController](https://github.com/FIRST-Tech-Challenge/FtcRobotController)
2. Follow instructions to fork the FtcRobotController repository:
   [Fork a repo — GitHub Docs](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/fork-a-repo)
3. Go to the newly forked repository under team GitHub.
4. Click **Code -> Open with GitHub Desktop**.
5. Choose **Open in Android Studio**.

Now your project is connected to both your GitHub remote repository and the official FtcRobotController remote repository.

### Get Updates from the FTC FtcRobotController Repository (Android Studio Steps)

1. Checkout the `main` branch.
2. Choose **FtcRobotController…master -> master…origin/master -> update**.

---

## Download Team GitHub Repository (by Team Members)

> **Warning:** Make sure your GitHub folder is **not** stored in a Microsoft OneDrive folder — we need to edit it offline.

### GitHub Steps

1. Accept the invitation (check your inbox).
2. Go to our GitHub repositories:
   - [https://github.com/duckyduckies/INTOTHEDEEP/](https://github.com/duckyduckies/INTOTHEDEEP/)
   - [https://github.com/duckyduckies/Training/](https://github.com/duckyduckies/Training/)
   - [https://github.com/duckyduckies/Training2025/](https://github.com/duckyduckies/Training/)
3. Click **Code -> Local -> Open with GitHub Desktop**.
4. In the GitHub Desktop app, click **Clone**.
5. Choose **Open in Android Studio**.

---

## Install FTC Dashboard

1. Follow the [FTC Dashboard Getting Started guide](https://acmerobotics.github.io/ftc-dashboard/gettingstarted).
   Follow the **Basic** sections and ControlHub instructions to edit and connect to the control hub.

2. *(Already done for the Training 2025 project)* Copy and test this camera program using FTC Dashboard:
   [VisionPortalStreamingOpMode.java](https://github.com/acmerobotics/ftc-dashboard/blob/master/TeamCode/src/main/java/org/firstinspires/ftc/teamcode/VisionPortalStreamingOpMode.java)

3. Check the [FTC Dashboard Features doc](https://acmerobotics.github.io/ftc-dashboard/features) for configuring and showing variables, including:
   - Telemetry
   - Configuration Variables

---

## Option A: Download the Team Code

### Android Studio Steps

1. Checkout the `master` branch at the top.
2. **Git -> Pull** -> Choose the remote `origin/master` branch. *(Gets the latest code.)*
3. **Git -> New Branch** -> Enter the task name `T` (consider including your name and date).
4. Now you can edit the code.

---

## Option B: Create Your Own Code from the Sample Programs

### Android Studio Steps

1. **Git -> New Branch** -> Enter the task name `T` (consider including your name).
2. Copy the three programs below from `FTCRobotController/java/org.firstinspires.ftc.robotcontroller/external.samples`:
   - Paste to `TeamCode/java/org.firstinspires.ftc.teamcode`
   - Click **Add** & **OK**

### Sample Programs from FTCRobotController


- [ ] **BasicOpMode_Linear.java**
  1. Structured sequentially.
  2. Drives a two-motor drivetrain in POV style.

- [ ] **RobotTeleopPOV_Linear.java**
  1. Structured sequentially.
  2. Drives a two-motor drivetrain in POV style.
  3. Also controls an arm motor and two claw servos.
  4. Our team's first robot used this structure.

- [ ] **ConceptExternalHardwareClass.java** + **RobotHardware.java**
  1. Structured in an object-oriented fashion. `RobotHardware` is the robot class; `ConceptExternalHardwareClass` is the master program.
  2. Drives a two-motor drivetrain in POV style.
  3. Also controls an arm motor and two claw servos.
  4. Our team's second and third robots used this structure.

3. Edit the actuators' names and configure them on the Driver Hub accordingly.

---

## Locally Commit the Program

### Android Studio Steps

1. **Git -> Commit**
2. On the left side, record what you have modified.
3. Click **Commit** *(not "Commit and Push" — we don't want to overwrite others' programs).*

---

## Test the Program on the Robot

### Android Studio Steps

1. **Build -> Make Project**
2. Turn on the robot and Driver Hub.
3. On both the Driver Hub and computer, connect to Wi-Fi `24175-RC` or `ftc-xxxx` with password `slayducky1`.
   *(You may also connect the computer to the Driver Hub using a USB-C cable instead of Wi-Fi.)*
4. Run the REV Hardware Client software — make sure it detects the control hub of the robot.
5. Click the **Run "TeamCode"** button, or use the menu: **Run -> Run**. *(This deploys the code.)*

### Driver Hub Steps

1. Open the **FTC DriverStation** app.
2. Tap the three-dot menu -> **"Configure Robot"**.
3. Edit and activate the **"ChannelDrivetrain"** (or other) configuration.
4. Choose a TeleOp program to run.

> The Driver Station app on the Driver Hub can store groups of programs and provides two menus (TeleOp or Autonomous) to choose from. The master structure of a TeleOp program is a loop that takes gamepad input at each iteration and moves the robot. See *"02 Program Servos & Motors"* for details.

---

## Rebase with the Latest Remote Repository
before uploading the program

### Android Studio Steps

To update your `T` branch with the latest `master` code from remote (which may have been changed by others):

**Method 1 — Updates both local `master` and local `T` branches:**
1. Checkout the `master` branch at the top.
2. **Git -> Pull**
3. Checkout the `T` branch at the top.
4. **Git -> Rebase**
5. Choose the `master` branch (the local one).

**Method 2 — Easier; updates only the local `T` branch:**
1. **Git -> Fetch**
2. Checkout the `T` branch at the top.
3. **Git -> Rebase**
4. Choose the `origin/master` branch (the remote one).
5. Solve any conflicts.

Now you can upload your code.

---

## Upload the Program

### Android Studio Steps

**Option A: Upload to the remote `T` branch:**
1. *(Assume you already committed your code on branch `T`.)*
2. **Git -> Push**
3. Choose the remote `T` branch.

**Option B: Push to the `master` branch** 
*(only after the program is tested by the team!)*:

If the remote `master` branch has been updated by others after you rebased:
1. *(Assume you already committed and rebased branch `T` with the latest `master`.)*
2. Checkout the `master` branch at the top.
3. **Git -> Merge**, choose `T`.

If merging fails:
1. Checkout the `T` branch at the top.
2. **Git -> Rebase**
3. Solve conflicts in the code.
4. Go back to step 1 of merging.

Upload to master:
1. Checkout the `master` branch at the top.
2. **Git -> Push**

If the remote `master` branch is unlikely to have been updated after you rebased:
1. Checkout the `T` branch at the top.
2. **Git -> Push**
3. Choose the remote `origin/master` branch.

---

### Troubleshooting: Push Is Denied in Android Studio

**If you cannot push using GitHub Desktop:** 
Make sure the owner has added your account to the repository with **Write** access, and that you have accepted the invite.

**If you can push using GitHub Desktop but not Android Studio:** 
1. Follow the instructions at [Stack Overflow: Can't log in to GitHub on Android Studio](https://stackoverflow.com/questions/64869735/cant-log-in-to-github-on-android-studio).
2. If that doesn't work, check out the Windows Credential Manager or Git Bash & SSH key methods.

---

## Use Git Bash Instead of GitHub (Optional)

### Git Commit Procedure

```bash
git stash           # Store your changes
git pull --rebase   # Get latest code from repository
git stash apply     # Apply your changes (fix conflicts here if any)
git add <files>     # Stage your files
git commit          # Commit (write a message, then hit Esc and type :wq)
git push            # Final push
```

### Git Branch Commands

```bash
git branch yourBranchName
git checkout yourBranchName
# Create branch on GitHub, then:
git push --set-upstream team_repo yourBranchName
```

### Important Git Commands Reference

| Command | Description |
|---|---|
| `git status` | Check your changes |
| `git branch` | See which branch you are on |
| `git remote -v` | List the remote repositories |
| `git checkout <filename>` | Discard a local change, restore from git |
| `git checkout HEAD <filename>` | Restore a file that was accidentally deleted |
