# Dialogue System

Last Updated October, 9, 2024

## Overview
This will cover how our dialogue system is implemented, reads files, and displays text onto the frontend.
The following scripts will be explained and how their methods work.
- DIALOGUE_LINE
- FilePaths
- FileManager
- DialogueSystem
- ConversationManager
- DialogueParser
- DialogueContainer
- NameContainer
- TextArchitect

## DIALOGUE_LINE
This script is a model for parsed lines from text files.

Once a line is parsed, speaker information and the dialogue are stored into this object model.
The model has Bools to check if it has a speaker, dialogue, or is a command. 

## FilePaths
A read only script holding the path of the root directory to all the text files.

## FileManager

The `FileManager` class provides utility functions to read text files and text assets within a Unity project. It supports both direct file access and access to text files as Unity assets, and includes options to include or exclude blank lines during reading.

### Public Methods

#### `ReadTextFile(string filePath, bool includeBlankLines = true)`


Reads the contents of a text file from a specified path and returns a list of lines.

##### Parameters
- `filePath` (`string`): The relative or absolute path to the file. If the path does not start with `/`, the method prepends a root path.
- `includeBlankLines` (`bool`): Whether to include blank lines in the returned list. Defaults to `true`.

##### Returns
- `List<string>`: A list of lines read from the file, optionally excluding blank lines.

##### Example

```csharp
List<string> lines = FileManager.ReadTextFile("file.txt", false);## FileManager
FileManager opens a text file and returns each line

ReadTextFile(string filePath, bool includeBlankLines = true)

filePath should be the name of the text file as the method references the root directory to the text files in FilePaths.
includeBlankLines is defaulted to true, setting it to false will ignore blank lines in the text file and will not return
them.

This method will read the entire text file and appends each line into a Lis<string>;
```
---

#### `ReadTextAsset(string filePath, bool includeBlankLines = true)`


Reads a TextAsset from Unity's Resources folder and returns its lines in a list.

##### Parameters
- `filePath` (`string`): The path to the `TextAsset file` in the `Resources` folder, without the file extension.
- `includeBlankLines` (`bool`): A flag to specify whether to include blank lines in the returned list. Defaults to `true`.

##### Returns
- `List<string>`: A list containing the lines from the `TextAsset`, optionally excluding blank lines.

##### Example

```csharp
List<string> lines = FileManager.ReadTextAsset("file.txt", false)

filePath should be the name of the text file as the method references the root directory to the text files in FilePaths.
includeBlankLines is defaulted to true, setting it to false will ignore blank lines in the text file and will not return
them.

This method will read the entire text file and appends each line into a Lis<string>;
```
---
#### `ReadTextAsset(TextAsset asset, bool includeBlankLines = true)`

Reads a TextAsset from Unity's Resources folder and returns its lines in a list.

##### Parameters
- `filePath` (`string`): The path to the `TextAsset file` in the `Resources` folder, without the file extension.
- `includeBlankLines` (`bool`): A flag to specify whether to include blank lines in the returned list. Defaults to `true`.

##### Returns
- `List<string>`: A list containing the lines from the `TextAsset`, optionally excluding blank lines.

##### Example

```csharp
[SerializedField] private TextAsset fileName;
List<string> lines = FileManager.ReadTextAsset(fileName, false)

## Ensure the file is attached in the editor

This method will read the entire text file and appends each line into a Lis<string>;
```
---

## DialogueSystem
The `DialogueSystem` class is part of the `DIALOGUE` namespace in Unity and handles the dialogue interactions in the game. It manages conversations, speaker names, and user prompts for advancing dialogue. It is designed to work with `TextArchitect` and `ConversationManager` for building and managing the dialogue flow.
### Public Properties

#### `DialogueContainer dialogueContainer`

An instance of the `DialogueContainer` class that holds the UI elements such as the dialogue text and speaker name display.

---

#### `bool isRunningConversation`

A property that returns whether a conversation is currently running in the dialogue system, as determined by the `ConversationManager`.

---
### Public Methods
#### `void OnUserPrompt_Next()`

Triggers the next step in the dialogue when the user prompts the system. It invokes the `onUserPrompt_Next` event to proceed to the next dialogue line.

##### Example Usage

```csharp
DialogueSystem.instance.OnUserPrompt_Next();
## will prompt the next line to display
```
---

#### `void ShowSpeakerName(string speakerName = "")`

Displays speaker name into the name container. If the narrator is speaking, the name will not be displayed.

##### Paramerters
- `speakerName` (`string`): string of the character speaking

##### Example Usage

```csharp
DialogueSystem dialogueSystem => DialogueSystem.instance;

dialogueSystem.ShowSpeakerName("Larry");
```
---

#### `void HideSpeakerName()`

Hides the speaker name in the name container.

##### Example Usage

```csharp
DialogueSystem dialogueSystem => DialogueSystem.instance;

dialogueSystem.HideSpeakerName();
```
---

#### `void Say(string speaker, string dialogue)`

This method will combine speaker's name and their dialogue into one string. Using the `ConversationManager` it will begin building and displaying text into the dialogue container.

##### Parameters

- `speaker` (`string`): name of the speaker
- `dialogue` (`string`): dialogue to be displayed

##### Example Usage

```csharp

DialogueSystem.instance.Say("Larry", "A quick fox");
```
---

#### `void Say(List<string> conversation)`
Using the `ConversationManager` it will begin building and displaying  text into the dialogue container.

##### Parameters
- `conversation` (`List<string>`): list of conversations to be displayed

##### Example Usage
```csharp
List<string> lines = FileManager.ReadTextAsset(fileName, false);

DialogueSystem.instance.Say(lines);
```
---


