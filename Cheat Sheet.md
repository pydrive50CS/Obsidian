# Obsidian Cheat Sheet (Linux)

## 1. Core Navigation

- **Open Command Palette**: `Ctrl + P`
    
- **Quick Switcher (Jump to Note)**: `Ctrl + O`
    
- **Open File Explorer**: `Ctrl + Shift + E`
    
- **Omni Vault Search**: `Ctrl + Shift + F`
    
- **Back / Forward Navigation**: `Ctrl + Alt + Left / Right`??
    

---

## 2. Note Editing

- **Create New Note**: `Ctrl + N`
    
- **Toggle Edit/Preview Mode**: `Ctrl + E`
    
- **Open Current Note in New Pane**: `Ctrl + Shift + Enter`
    
- **Split Pane Horizontally**: `Ctrl + Shift + Right`
    
- **Close Current Pane**: `Ctrl + W`

---

## 3. Linking & Backlinks (Super Important)

### Internal Links

- **Create Wiki Link**: `[[Note Name]]`
    
- **Link to Header**: `[[Note Name#Header]]`
    
- **Link to Block**: `[[Note Name^block-id]]`
    

### Embed Notes / Elements

- **Embed Note**: `![[Note Name]]`
    
- **Embed Heading in Note**: `![[Note Name#Heading]]`
    
- **Embed Block**: `![[Note Name^block-id]]`
    

### Backlinks Panel

- Toggle backlinks for current note: `Ctrl + Shift + B`
    

---

## 4. Tags

- **Add Tag**: `#tag`
    
- **Nested Tags**: `#topic/subtopic`
  
---

## 5. Formatting (Markdown)

- **Bold**: `Ctrl + B`
    
- **Italic**: `Ctrl + I`
    
- **Highlight**: `==text==`
    
- **Inline Code**: `` `code` ``
    
- **Code Block**:
    
    ````
    ```language
    code here
    ````
    
- **Blockquote**: `> Text`
    
- **Checkbox Tasks**:
    
    - To-do: `- [ ] Task`
        
    - Done: `- [x] Task`
        
    - In Progress: `- [-] Task`
        

---

## 6. Search

- **Global Search**: `Ctrl + Shift + F`
    
- **Search Current File**: `Ctrl + F`
    
- **Use Regex**: Toggle `.*` in search panel
    

**Search Filters:**

- Tag: `tag:#tagname`
    
- File name: `file:"note name"`
    
- Path: `path:"foldername"`
    
- Line contains: `line:(text)`
    
- Tasks only: `task:"keyword"`
    

---

## 7. Useful Hotkeys

- **Toggle Sidebar**: `Ctrl + Shift + S`
    
- **Open Settings**: `Ctrl + ,`
    
- **Toggle Preview Mode**: `Ctrl + E`
    
- **Markdown Heading Levels**:
    
    - H1: `Ctrl + 1`
        
    - H2: `Ctrl + 2`
        
    - H3: `Ctrl + 3`
        
    - ... up to H6
        
- **Toggle Bullet List**: `Ctrl + L`
    
- **Toggle Numbered List**: `Ctrl + Shift + L`
    
- **Indent / Outdent**: `Tab` / `Shift + Tab`
    

---

## 8. Dataview (If Installed)

- **Inline Query**: `= this.file.name`
    
- **Codeblock Query**:
    
    ````
    ```dataview
    TABLE field1, field2
    FROM "folder"
    ````
    

---

## 9. Canvas (If Installed)

- Create a canvas: Right-click → _New Canvas_
    
- Add cards: Double-click canvas
    
- Link cards with drag arrow
    

---

## 10. Daily Notes / Templates

- **Open Today's Note** (if plugin enabled): `Ctrl + Shift + D`
    
- **Insert Template**: `Ctrl + T`
    

---

## 11. Graph View

- **Open Graph View**: `Ctrl + Shift + G`
    
- **Local Graph for Note**: Right-click → _Open local graph_
    

---

## 12. File & Vault Management

- **Open Another Vault**: `Ctrl + Shift + O`
    
- **Move Note**: Drag in file explorer or use _Move file to…_
    

---

## 13. Advanced Linking Tips

- **Alias Linking**:  
    Add at top of a note:
    
    ```yaml
    ---
    aliases: ["alt name", "another"]
    ---
    ```
    
- Then link using: `[[alt name]]`
    
- **Block IDs**: Add block ID to any paragraph:  
    `^unique-id`
    
- **Callouts**:
    
    ```
    > [!note]
    > Important content here.
    ```
    

---
Adding a checklist in **Obsidian** is straightforward because Obsidian supports **Markdown**, and Markdown has built-in syntax for task lists. Here’s how you can do it:

---

### **1. Basic Checklist Syntax**

Use `- [ ]` for an unchecked item and `- [x]` for a checked item:

```markdown
- [ ] Buy groceries
- [ ] Call Alice
- [x] Submit report
```

This will render as:

-  Buy groceries
    
-  Call Alice
    
-  Submit report
    

---

### **2. Nested Checklists**

You can create subtasks by indenting with spaces or tabs:

```markdown
- [ ] Project
    - [ ] Write proposal
    - [x] Send email
```

Renders as:

-  Project
    
    -  Write proposal
        
    -  Send email
        

---

### **3. Tips**

- You can **toggle checkboxes** directly in Obsidian preview mode by clicking on the box.
    
- Checklists can be combined with **headings** to organize tasks:
    

```markdown
## Today
- [ ] Morning meeting
- [ ] Review PRs

## Tomorrow
- [ ] Team sync
```

- Plugins like **Tasks** or **Checklist Plugin** allow advanced features like due dates, recurring tasks, and queries across notes.

---
## **1. Create a Task**

Use the plugin’s extended Markdown syntax:

```markdown
- [ ] Buy groceries 🏷️ #shopping 📅 2025-12-21 🔁 every week
- [ ] Call Alice 📅 2025-12-22 🔁 every month
- [ ] Submit report 📅 2025-12-21 ⏳ high
```

**Explanation:**

- `- [ ]` → Task (unchecked)
    
- `#tag` → Optional tag
    
- `📅 YYYY-MM-DD` → Due date
    
- `⏳ priority` → Optional priority (`low`, `medium`, `high`)
    
- `🔁 every X` → Recurring task
    

---

## **3. Query Tasks Across Notes**

You can create a **task query block** to list tasks dynamically:

````markdown
```tasks
not done
due before 2025-12-22
tag includes #shopping
sort by due
````

```

This block will automatically list all tasks:
- That are **not done**  
- **Due before 22 Dec 2025**  
- Tagged with `#shopping`  
- Sorted by due date  

You can adjust filters like `done`, `not done`, `priority`, `tag includes`, etc.

---

## **4. Marking Tasks as Done**
- In **Preview mode**, just click the checkbox to mark it as complete.  
- Recurring tasks will automatically generate the next occurrence if you set a recurrence.

---

✅ **Tip:** You can combine Tasks plugin with **Daily Notes** to have a daily to-do list that automatically pulls in all due tasks.  

---

If you want, I can make a **ready-to-use template for Obsidian daily tasks** using the Tasks plugin, including priorities, due dates, and recurring tasks.  

Do you want me to make that template?
```
---
Shift+Alt+Drag  is multi cursor

