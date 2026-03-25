# Lab 1: Integrate Firebase to an HTML file
## Main Contents
### [Lab 1](#lab-1-integrate-firebase-to-an-html-file)
### [Lab 2](#lab-2-creating-a-simple-student-recording-website)

## Content
* [Setting up NPM](#setting-up-the-node-package-manager)
* [Tips to following Tutorial](#tips-on-following-this-tutorial)
* [Start of Tutorial](#start-of-tutorial)
* [Step 1](#step-1)
* [Step 2](#step-2)
* [Step 3](#step-3)



## Setting up The Node Package Manager
For devices with npm already installed, the latest version can be installed on the command line with\
`npm install -g npm`\
To check if you have Node.js already installed, use the commands\
`npm -v`\
`node -v`\
an error here means that node is not installed
### Installation
For people who want to download via the node version manager, the repository for NVVM are the following:
### MACOS and Linux
* [NVM](https://github.com/nvm-sh/nvm)
* [n](https://github.com/tj/n)
### Windows 
* [nodist](https://github.com/marcelklehr/nodist)
* [nvm-windows](https://github.com/coreybutler/nvm-windows)
### Using a node installer for both Node.js and NPM 
* [Node.js installer](https://nodejs.org/en/download/https://nodejs.org/en/download/)


## Tips on Following this Tutorial
- ensure that npm is installed
- be familiar with html and javascript
- have a firebase account
- DO NOT SHARE your API Config to anyone \(If you dont want to be charged with 1 Morbillion dollars for fees)

# 行きましょう!

## Start of Tutorial
<img width="1657" height="546" alt="Firestore (1)" src="https://github.com/user-attachments/assets/3a59aa4e-3b52-429c-affd-8e50d8224ada" />
Go to the firebase console and create a project (or use existing ones) for this tutorial

## Step 1
<img width="1084" height="583" alt="Firestore" src="https://github.com/user-attachments/assets/4ba94571-94bd-4f47-92d3-b44a968ad34b" />
Go to the general tab and press the html tag icon

## Step 2
<p align="center">
  <img width="804" height="695" alt="Firestore (3)" src="https://github.com/user-attachments/assets/66f77c9d-c662-48af-b944-35b0f259e6a1" />
</p>
Add information of this app in this (also check the Firebase Hosting if you want this app to go public)

## Step 3
<p align="center">
  <img width="625" height="705" alt="Firestore (4)" src="https://github.com/user-attachments/assets/b441fb99-cb38-4094-b6e2-4472e3c605a7" />
</p>
Copy the <scrip> tag located at the top image
  <p align="center">
    <img width="293" height="114" alt="image" src="https://github.com/user-attachments/assets/5b2cdbba-6ef7-4ce6-8806-3a083a2878e7" />
  </p>
Paste it in an html file in your directory

**Now the file will serve as your webpoage for the firebase lab**

# LAB 2: Creating A Simple Student Recording Website
## Content
- [scenario](#scenario)
- [demo](#start-of-demo)
  
## Scenario
The Halimaw University needs to have a student record system to list their halimaw students (They are very skillful in their fields). You are tasked to create the system with the following technologies: 
1. Firebase
2. html
3. javascript
4. css

### Your tasks are to:

- [ ] ✅ initialize firebase and html file

- [ ] alllow adding of students using forms

- [ ] allow deletion of students

- [ ] allow for updating the age and program of the student

- [ ] allow listing of courses that the students have enrolled to

- [ ] allow for adding and deletion of courses

- [ ] allow for displaying these in a neat and readable manner


## Helper Notes
The helper seems to have created the base code, and had indexed it for you
### Table of Contents
1. [Initialize HTML Structure](#1-initialize-html-structure)
2. [Firestore Integration](#2-firestore-integration)
3. [Adding Students](#3-adding-students-create)
4. [Reading Data](#4-reading-data--sub-collections-read)
5. [Update & Delete](#5-update--delete-operations)
   
The Halimaw University will pay you with 20 bitcoins for your troubles and had provided a helper for you to make the system.
## Start of Demo
### 1. Initialize HTML Structure  
First, we create the skeleton. We need inputs for student details and a "container" div where our data will be displayed.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Firebase Firestore CRUD</title>
    <style>
        body { font-family: sans-serif; padding: 20px; }
        .person-card { border: 1px solid #ccc; margin: 10px 0; padding: 15px; border-radius: 8px; }
        .course-section { margin-left: 20px; border-left: 3px solid #666; padding-left: 15px; background: #f9f9f9; }
    </style>
</head>
<body>
    <h2>Add New Student</h2>
    First Name: <input type="text" id="fn"><br>
    Last Name: <input type="text" id="ln"><br>
    Age: <input type="number" id="age"><br>
    Program: <input type="text" id="course"><br>
    <label><input type="checkbox" id="isHandsome" checked> Is Handsome</label><br>
    
    <button onclick="addToDB()">Add Student</button>

    <hr>
    <div id="content">Loading student list...</div>

    <script type="module">
        // MODULE CODE GOES HERE
    </script>
</body>
</html>
```

---

### 2. Firestore Integration 
Inside your `<script type="module">`, we import the necessary tools and initialize the connection using your unique config.

```javascript
import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
import { 
    getFirestore, collection, getDocs, addDoc, deleteDoc, updateDoc, doc 
} from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

const firebaseConfig = {
    // PASTE YOUR CONFIG FROM FIREBASE CONSOLE HERE
};

const app = initializeApp(firebaseConfig);
const db = getFirestore(app);
const peopleRef = collection(db, "People");
```

---

### 3. Adding Students (Create) 
To add people, we gather the input values and send them to the `People` collection. We attach this to `window` so the HTML button can "see" it.

```javascript
window.addToDB = async () => {
    const firstName = document.getElementById("fn").value;
    const lastName = document.getElementById("ln").value;
    const age = document.getElementById("age").value;
    const isHandsome = document.getElementById("isHandsome").checked;
    const program = document.getElementById("course").value;

    try {
        await addDoc(peopleRef, {
            Full_Name: `${firstName} ${lastName}`,
            Age: Number(age),
            isHandsome: isHandsome,
            Program: program
        });
        getPeopleList(); // Refresh the list automatically
    } catch (err) { console.error(err); }
};
```

---

### 4. Reading Data & Sub-collections (Read) 
We fetch the students, and for **each student**, we fetch their nested `attendedCourses` collection.

```javascript
async function getPeopleList() {
    const contentDiv = document.getElementById("content");
    const data = await getDocs(peopleRef);
    
    const peopleData = await Promise.all(data.docs.map(async (personDoc) => {
        // Fetch Sub-collection for this specific person
        const coursesRef = collection(db, "People", personDoc.id, "attendedCourses");
        const snapshot = await getDocs(coursesRef);
        const courses = snapshot.docs.map(cDoc => ({ ...cDoc.data(), course_id: cDoc.id }));
        
        return { ...personDoc.data(), id: personDoc.id, courses };
    }));

    renderUI(peopleData);
}

function renderUI(peopleData) {
    const contentDiv = document.getElementById("content");
    let html = "";
    peopleData.forEach(person => {
        html += `
            <div class="person-card">
                <h1>${person.Full_Name}</h1>
                <p>Age: ${person.Age} <button onclick="updateAge('${person.id}')">Update</button></p>
                <p>Program: ${person.Program} <button onclick="updateCourse('${person.id}')">Update</button></p>
                <button onclick="deletePerson('${person.id}')">Delete Student</button>
                
                <div class="course-section">
                    <h3>Enrolled Courses:</h3>
                    ${person.courses.map(c => `
                        <p><strong>${c.course}</strong> (${c.instructor}) 
                        <button onclick="deleteCourse('${person.id}', '${c.course_id}')">x</button></p>
                    `).join('')}
                    <button onclick="handleAddCourseClick('${person.id}')">+ Add Course</button>
                </div>
            </div>`;
    });
    contentDiv.innerHTML = html;
}
```

---

### 5. Update & Delete Operations 
Finally, we implement the logic to remove students or modify their details using `doc()` to point to the specific record.

```javascript
// DELETE STUDENT
window.deletePerson = async (id) => {
    await deleteDoc(doc(db, "People", id));
    getPeopleList();
};

// UPDATE AGE
window.updateAge = async (id) => {
    const newAge = prompt("Enter new age:");
    if (newAge) {
        await updateDoc(doc(db, "People", id), { Age: Number(newAge) });
        getPeopleList();
    }
};

// ADD COURSE (SUB-COLLECTION)
window.handleAddCourseClick = async (personId) => {
    const name = prompt("Enter course name:");
    if (!name) return;
    const coursesRef = collection(db, "People", personId, "attendedCourses");
    await addDoc(coursesRef, {
        course: name,
        instructor: prompt("Instructor name:") || "TBA",
        passed: true
    });
    getPeopleList();
};
```

