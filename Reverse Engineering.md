
# Inspection .
--
We are cleaning up GitHub a file at a time
Today's clean up sequence:
1. Inspect current Git state
```text
        ↓
```
2. Inspect repository structure
```text
        ↓
```
3. Identify what actually belongs in Git
```text
        ↓
```
4. Clean files / folders
```text
        ↓
```
5. Understand the code
```text
        ↓
```
6. Update README
```text
        ↓
```
7. Make meaningful commits
```text
        ↓
```
8. Push
```text
        ↓
```
9. Record the learning in Engineer's Laboratory Book

`Get this between your ears that before doing anything you check status so  first command is :
```powershell
git status
```
This was the output :
```powershell
On branch main
Your branch is up to date with 'origin/main'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        .idea/

nothing added to commit but untracked files present (use "git add" to track)
```

```text
Untracked files:
```
```text
  (use "git add <file>..." to include in what will be committed)
```
```text
        .idea/
```

nothing added to commit but untracked files present (use "git add" to track) '

The status is in a clean and safe state but there is an issue with .idea/.
What is .idea/?
That is IntelliJ IDEA's project configuration directory. It contains IDE-specific settings such as:

workspace configuration
local IDE preferences
project metadata
sometimes machine-specific paths

For a Java project, we generally do not want the whole .idea/ directory in Git.

This is exactly the kind of thing I am cleaning up.

# Step 1 :Let's inspect before making any changes.
ls -la .idea - powershell rejects since ls behaves differently in powershell and more differently in bash. Although they both carry the same meaning show the contents, including hidden files.

PowerShell:
```powershell
Get-ChildItem -Force
```
Output:
Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a---l         9/15/2026   3:42 PM             50 .gitignore
-a---l         9/15/2026   3:46 PM             24 .name
-a---l         9/15/2026   3:42 PM            344 commute.iml
-a---l         9/15/2026   3:42 PM            273 modules.xml
-a---l         9/15/2026   3:42 PM            172 vcs.xml
-a---l         9/15/2026   9:24 PM           1854 workspace.xml
**The most important discovery:**
```text
.idea/
```
```text
├── .gitignore
├── .name
├── commute.iml
├── modules.xml
├── vcs.xml
└── workspace.xml
```
**The verdict is that: I do not commit this directory. But why?**
For my cleanup, .idea/ is IDE metadata, not application source code.

In particular, workspace.xml can contain local IDE/workspace configuration. There's no reason for it to be part of my repository.

But we also don't need to manually delete .idea/ right now.

 # The next course of action is to inspect git ignore but take not of this first:
Run this command : .gitignore
We are specifically looking for a root-level :
```powershell
.gitignore
```
Notice the distinction :
CapitalCommute/
```text
├── .gitignore       ← THIS is what we want
└── .idea/
    └── .gitignore   ← this is IntelliJ's own file
```
We now inspecting the root. gitignore before making any changes
Output:
Directory: C:\Users\wanga\OneDrive\Desktop\commute

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
darh-l         9/17/2026   4:55 PM                .git
dar--l         9/15/2026   9:24 PM                .idea
dar--l          9/1/2026  10:25 PM                client
dar--l          9/1/2026  10:25 PM                config
dar--l          9/1/2026  10:25 PM                Controller
dar--l          9/1/2026  10:25 PM                DTO
dar--l          9/1/2026  10:25 PM                model
dar--l          9/1/2026  10:25 PM                repository
dar--l          9/1/2026  10:25 PM                Service
dar--l          9/1/2026  10:25 PM                UI
dar--l          9/1/2026  10:25 PM                util
-a---l         4/30/2026   7:40 PM           1259 CapitalCommut
                                                  eApp.class
-a---l         4/30/2026   7:40 PM            762 CommuteApplic
                                                  ation.class

Let's pay attention now : # 2 things stand out :
1. There is NO root .gitignore

That's why Git is reporting:

```text
Untracked files:
```
```text
    .idea/
```
**Resolution:** I ma going to create one.

2.I have .class files in the repository root

These:

CapitalCommuteApp.class
CommuteApplication.class

are compiled Java bytecode, not source code.

For a source-code GitHub repository,  normally don't track generated .class files.

- we are not going to delete anything first.

# What is actually tracked by Git?

Because the current directory may contain recovered/decompiled material, and I don't want us accidentally destroying evidence from the old project.
# Step 3 - Inspect what Git currently tracks
git status asks:

"What has changed or is untracked right now?"

git ls-files asks:

"What files does Git actually know about?"

You will never know what a very useful distinction  that is till when you will be cleaning an old repository.
git ls-files - This is now  a reconnaissance , like getting reacquainted with an old friend.
CapitalCommuteApp.class
CommuteApplication.class
Controller/AdminController.class
Controller/AuthController.class
Controller/BookingController.class
Controller/ClientController.class
Controller/DebugController.class
Controller/DriverController.class
Controller/FareCalculatorController.class
Controller/PaymentController.class
Controller/VehicleController.class
DTO/CaptchaResult.class
DTO/LoginRequest.class
DTO/LoginResponse.class
DTO/RegisterRequest.class
Service/AuthService.class
Service/BookingService.class
Service/ClientService.class
Service/DriverService.class
Service/FareCalculatorService.class
Service/PaymentService.class
Service/VehicleService.class
UI/AdminDashboard.class
UI/AdminLoginPanel.class
UI/BaseDashboard.class
UI/ClientDashboard.class
UI/DriverDashboard$1.class
UI/DriverDashboard$2.class
UI/DriverDashboard$3.class
UI/DriverDashboard$4.class
UI/DriverDashboard$5.class
UI/DriverDashboard$6.class
UI/DriverDashboard$7.class
UI/DriverDashboard$8.class
UI/DriverDashboard$9.class
UI/DriverDashboard.class
UI/GPSSimulationPanel$1.class
UI/GPSSimulationPanel$VehicleMarker.class
UI/GPSSimulationPanel.class
UI/LoginPanel.class
UI/RegistrationPanel.class
UI/components/BookingTablePanel$1.class
UI/components/BookingTablePanel$2.class
UI/components/BookingTablePanel$3.class
UI/components/BookingTablePanel$4.class
UI/components/BookingTablePanel.class
UI/components/NewBookingPanel.class
UI/components/PaymentTablePanel$1.class
UI/components/PaymentTablePanel$2.class
UI/components/PaymentTablePanel$3.class
UI/components/PaymentTablePanel$4.class
UI/components/PaymentTablePanel$5.class
UI/components/PaymentTablePanel.class
UI/components/ProfilePanel.class
UI/components/RideTablePanel$1.class
UI/components/RideTablePanel$2.class
UI/components/RideTablePanel$3.class
UI/components/RideTablePanel$4.class
UI/components/RideTablePanel.class
UI/components/UserTablePanel$1.class
UI/components/UserTablePanel$2.class
UI/components/UserTablePanel$3.class
UI/components/UserTablePanel.class
UI/components/VehicleTablePanel$1.class
UI/components/VehicleTablePanel$2.class
UI/components/VehicleTablePanel$3.class
UI/components/VehicleTablePanel.class
client/ApiClient.class
config/SecurityConfig.class
model/Admin.class
model/AdminProfile.class
model/Booking.class
model/Client.class
model/Driver.class
model/Payment.class
model/Person.class
model/Ride.class
model/Vehicle$VehicleBuilder.class
model/Vehicle.class
model/enums/BookingStatus.class
model/enums/DriverStatus.class
model/enums/Gender.class
model/enums/MaintenanceStatus.class
model/enums/PaymentMethod.class
model/enums/PaymentStatus.class
model/enums/PersonType.class
model/enums/RideStatus.class
model/enums/VehicleStatus.class
repository/BookingRepository.class
repository/ClientRepository.class
repository/DriverRepository.class
repository/PaymentRepository.class
repository/PersonRepository.class
repository/VehicleRepository.class
util/CaptchaGenerator.class
util/DialogUtil.class
util/GsonUtil.class
util/SceneManager.class

This is moving from bad to worse real quick ; the above is a critical piece of evidence that the  entire tracked repository is compiled .class files. There are zero .java source files tracked in this Git history.
What do this mean for me ; what does the repo tell me in plain words; The git repository is essentially a  :TCMS Git history
```text
        │
        └── compiled Java bytecode (.class)
                │
                ├── Controller
                ├── DTO
                ├── Service
                ├── UI
                ├── model
                ├── repository
                ├── config
                ├── client
                └── util
```

So this isn't simply a case of "clean up a few unnecessary files."

The repository's historical baseline is a compiled artifact.

And because I've recovered a separate source-code version of TCMS - with an old architectural design.

Version A : TCMS local folder : Capital Commute remote repo
The one we've been reverse-engineering: It has POJOs

dao/
dao/
database/
gui/
model/
service/
Main.java
...

Now we have the critical piece of evidence. 👀

Your entire tracked repository is compiled .class files. There are zero .java source files tracked in this Git history.

That explains a lot of the mess we've been dealing with.

What this tells us

Your Git repository is essentially:

TCMS Git history
```text
        │
        └── compiled Java bytecode (.class)
                │
                ├── Controller
                ├── DTO
                ├── Service
                ├── UI
                ├── model
                ├── repository
                ├── config
                ├── client
                └── util
```

So this isn't simply a case of "clean up a few unnecessary files."

The repository's historical baseline is a compiled artifact.

And because you've recovered a separate source-code version of TCMS, we have something much more valuable to work with: the source itself.

One important rule before we touch Git

We're going to preserve this historical state.

Your existing commit:

767c81a Initial commit: taxi app baseline

is evidence of what was originally committed.

We're not going to rewrite that history just because it isn't ideal.

Instead, our rehabilitation can look like:

767c81a
Initial commit: taxi app baseline
```text
        │
        │  historical compiled baseline
        ↓
```
75aab21
chore: establish recovered TCMS baseline
```text
        │
        ↓
```
[cleanup commits]
```text
        │
        ├── restore/organize source
        ├── add .gitignore
        ├── document architecture
        └── update README
```

That makes the repository's evolution understandable rather than pretending the messy history never happened.

But we need to answer one question first

We have two versions of this project:

Version B — Local folder Commute :TCMS remote Git repository

The compiled Springboot/JavaFX version:

Controller/
DTO/
Service/
UI/
client/
config/
```text
model/
```
repository/
util/

Let us summarise my discoveries or hints for reverse engineering:
The GitHub repository currently contains compiled Java .class files instead of the Java source code we need for a proper portfolio repository.

**Conclusion so far:** I have evidence of two different architectural generations of the project.
GitHub TCMS — current tracked version
JavaFX UI
```text
   ↓
```
Controllers
```text
   ↓
```
Services
```text
   ↓
```
Repositories
```text
   ↓
```
Models

with additional pieces such as:

ApiClient
SecurityConfig
DTOs
CAPTCHA
Payment
GPS simulation
**Note:** And Git tracks the compiled .class representation of this version.

Recovered source version :Capital Commute Repo

The source you found has a different structure:

```text
GUI
 ↓
```
Services
```text
 ↓
```
DAOs
```text
 ↓
```
Database

with:

```text
model
```
dao
database
gui
service

This means i have parallel versions or generations of the projects . Some are in the Victorian era others in the Art deco era - o they cannot be merged.

I can now say:

"I have multiple recovered generations of the same project and need to establish their lineage before consolidating the repository."

That's an engineering problem, not a mess to blindly clean up.

So our immediate objective changes

We need to build a version map:

                    CAPITAL COMMUTE / TCMS
```text
                              │
                ┌─────────────┴─────────────┐
                │                           │
```
        Recovered Source              GitHub TCMS
        Architecture A                Architecture B
```text
                │                           │
```
        DAO/database/gui              Controller/DTO/
        service/model                 Service/Repository/UI
```text
                │                           │
                └────────── ? ──────────────┘
                           │
```
                    Are they related?
```text
                           │
```
                    establish evidence
