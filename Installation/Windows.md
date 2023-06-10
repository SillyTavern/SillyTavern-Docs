---
order: 10
label: Windows
---
# Windows Installation


!!!warning
DO NOT INSTALL INTO ANY WINDOWS CONTROLLED FOLDER (Program Files, System32, etc).

DO NOT RUN START.BAT WITH ADMIN PERMISSIONS
!!!

## Installing via GitHub Desktop (easiest)

  1. Install [NodeJS](https://nodejs.org/en) (latest LTS version is recommended)
  2. Install [GitHub Desktop](https://central.github.com/deployments/desktop/desktop/latest/win32)
  3. After installing GitHub Desktop, click on `Clone a repository from the internet....` (Note that you **do NOT need** to create an account for this step)

  ![image](https://github.com/SillyTavern/SillyTavern-Docs/assets/18619528/fae5d105-449a-4e4a-b679-238347647054)

  4. On the menu, go into the URL tab, enter this URL `https://github.com/SillyTavern/SillyTavern` and click Clone. You can change the Local path to change where SillyTavern is going to be downloaded.

  ![image](https://github.com/SillyTavern/SillyTavern-Docs/assets/18619528/6bdded9b-e182-4cac-89ff-e4727dbb97b2)

  6. To open SillyTavern, just go into the folder where you cloned the repository, and double click on the start.bat file. By default, the repository will be cloned here: `C:\Users[Your Account Name]\Documents\GitHub\SillyTavern`

  ![image](https://github.com/SillyTavern/SillyTavern-Docs/assets/18619528/a77b8bc2-72a9-42a9-8aa9-1ed89f9bbf35)

  6. If everything is working, the CMD should look like this and a SillyTavern tab should be open in your browser:

  ![image](https://github.com/SillyTavern/SillyTavern-Docs/assets/18619528/d9da4608-94cd-447c-bde2-7f0f9de1c2eb)

  7. Connect to any of the supported APIs and start chatting!

## Installing via Git

  1. Install [NodeJS](https://nodejs.org/en) (latest LTS version is recommended)
  2. Install [Git for Windows](https://gitforwindows.org/)
  3. Open Windows Explorer (`Win+E`)
  4. Browse to or Create a folder that is not controlled or monitored by Windows. (ex: C:\MySpecialFolder\)
  5. Open a Command Prompt inside that folder by clicking in the 'Address Bar' at the top, typing `cmd`, and pressing Enter.
  6. Once the black box (Command Prompt) pops up, type ONE of the following into it and press Enter:
  - for Main Branch: `git clone https://github.com/SillyTavern/SillyTavern -b main`
  - for Dev Branch: `git clone https://github.com/SillyTavern/SillyTavern -b dev`
  7. Once everything is cloned, double click `Start.bat` to make NodeJS install its requirements.
  8. The server will then start, and SillyTavern will popup in your browser.

## Installing via ZIP download (discouraged)

  1. Install [NodeJS](https://nodejs.org/en) (latest LTS version is recommended)
  2. Download the zip from this GitHub repo. (Get the `Source code (zip)` from [Releases](https://github.com/SillyTavern/SillyTavern/releases/latest))
  3. Unzip it into a folder of your choice
  4. Run `Start.bat` via double-clicking or in a command line.
  5. Once the server has prepared everything for you, it will open a tab in your browser.
