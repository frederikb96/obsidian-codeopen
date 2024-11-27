# Obsidian CodeOpen Integration for Visual Studio Code

This repository automates the addition of a new application via a `.desktop` file and registers the `.codeopen` extension. This allows you to quickly create and open a folder in Visual Studio Code based on a `.codeopen` file. It's particularly useful for seamlessly integrating this functionality with tools like Obsidian for note-taking and project management. The `.codeopen` file is used as a very lightweight and sustainable way to let Obsidian reliably create and open folders in Visual Studio Code. **Linux only**

## Usage

Run the playbook to set up the application:

```bash
ansible-playbook main.yml
```

The playbook performs the following:
1. Copies the `.desktop` file to register the new application.
2. Adds the MIME type for `.codeopen` files.
3. Updates the desktop and MIME databases to register the changes.

The setup is non-destructive, as it only adds new files and registers the `.codeopen` extension.

---

## Post Setup to Make It Work with Obsidian

### Step 1: Install the Templater Plugin

Install the [Templater plugin](https://github.com/SilentVoid13/Templater) in Obsidian. This plugin allows you to create templates with customizable actions.

### Step 2: Create a New Template File

Create a new template file in Obsidian with the following content:

```javascript
<%*
const userInput = await tp.system.prompt("Enter Code Project Name");
if (userInput) {
    
    // Create the full filename by appending the user input with the extension
    const fullFileName = `${userInput}.codeopen`;
    
    // Get the directory of the current note
    const currentDir = tp.file.folder(true);
    
    // Define the path to the attachments folder in the current note's directory
    const attachmentFolder = `${currentDir}/attachments`;
    
    // Full file path for the new file
    const fullFilePath = `${attachmentFolder}/${fullFileName}`;
    
    // Create the new file with a newline as content
    await this.app.vault.create(fullFilePath, "\n");
    
    // Insert a relative link to the new file in the current note
    const relativePath = `attachments/${fullFileName}`;
    tR += `[[${relativePath}]]`;
}
%>
```
We assume that the **new folder should be place in the attachments folder** of the current note. The template prompts you to enter a folder name, creates a new `.codeopen` file in the attachments folder, and inserts a relative link to the file in the current note.

### Step 3: Configure a Shortcut in Obsidian

In Obsidian, go to **Settings > Templater > Templates Folder** and set the folder containing your new template. Assign a shortcut to the template file for quick access.

### Step 4: Usage in Obsidian

1. Use the assigned shortcut to create a new `.codeopen` file in the `attachments` folder of the current note.
2. The template automatically inserts a relative link to the file in the current note.
3. Clicking the link opens the `.codeopen` file with the default system application, which triggers the new application registered in this repository.
4. The registered application creates a new folder named after the `.codeopen` file (without the extension) and opens the folder in Visual Studio Code.

---

This setup allows you to effortlessly create a new VS Code folder and project with a single click in Obsidian, while maintaining a direct link to the project in your notes.