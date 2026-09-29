<div align="center">

# 📸 SNAPCLASS

### AI-Powered Attendance System

<p>
  <b>Automated classroom attendance using Face Recognition + Voice Recognition.</b>
</p>

<p>
  <a href="https://snapclass-main-2ldavrzrff4rtafhtz3xii.streamlit.app/">
    <img src="https://img.shields.io/badge/🚀_Live_App-5865F2?style=for-the-badge&logo=streamlit&logoColor=white" alt="Live App">
  </a>
  <a href="https://github.com/Aayesha2103/snapclass-main">
    <img src="https://img.shields.io/badge/GitHub-111827?style=for-the-badge&logo=github&logoColor=white" alt="GitHub">
  </a>
</p>

<p>
  <img src="https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white">
  <img src="https://img.shields.io/badge/Streamlit-FF4B4B?style=flat-square&logo=streamlit&logoColor=white">
  <img src="https://img.shields.io/badge/Supabase-3ECF8E?style=flat-square&logo=supabase&logoColor=white">
  <img src="https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white">
  <img src="https://img.shields.io/badge/scikit--learn-F7931E?style=flat-square&logo=scikit-learn&logoColor=white">
</p>

</div>

---

## ✨ What is SnapClass?

**SnapClass** is an AI-powered classroom attendance system that automates attendance through **facial recognition** and **voice recognition**.

It provides separate **Teacher** and **Student** portals for managing subjects, student enrollment, biometric profiles, attendance, and attendance records.

### 🎯 Core Features

| 👨‍🏫 Teacher | 👨‍🎓 Student |
|---|---|
| 🔐 Register & Login | 📸 Face ID Login |
| 📚 Create & Manage Subjects | 🆕 New Profile Registration |
| 📸 Face Attendance | 🎙️ Optional Voice Enrollment |
| 🎙️ Voice Attendance | 🔗 Subject Code Enrollment |
| 🔳 Share Subject QR | 📊 View Enrolled Subjects |
| 📊 Attendance Records | 📈 View Attendance Stats |

---

## 🧠 AI Pipeline

### 📸 Face Recognition

```text
Classroom Image
      ↓
Face Detection (Dlib)
      ↓
128-D Face Embedding
      ↓
SVC Classifier
      ↓
Student ID Prediction
      ↓
Distance Threshold Check
      ↓
Attendance
```

### 🎙️ Voice Recognition

```text
Classroom Audio
      ↓
Librosa Audio Processing
      ↓
Speech Segmentation
      ↓
Resemblyzer Voice Embedding
      ↓
Similarity Matching
      ↓
Student Identification
      ↓
Attendance
```

---

## 🛠️ Tech Stack

<div align="center">

| Layer | Technologies |
|---|---|
| **Language** | Python |
| **UI / App** | Streamlit |
| **Landing Page** | Flask + Jinja + HTML/CSS |
| **Face AI** | Dlib + face_recognition_models |
| **ML Classifier** | scikit-learn SVC |
| **Voice AI** | Resemblyzer + Librosa |
| **Data Processing** | NumPy + Pandas + Pillow |
| **Database** | Supabase + PostgreSQL |
| **Security** | bcrypt |
| **QR Generation** | Segno |
| **Deployment** | Streamlit Cloud + Vercel |
| **Version Control** | Git + GitHub |

</div>

---

## 🏗️ Architecture

```text
                    SNAPCLASS
                       │
          ┌────────────┴────────────┐
          │                         │
     Teacher Portal           Student Portal
          │                         │
          └────────────┬────────────┘
                       ↓
                 AI Pipelines
              ┌────────┴────────┐
              │                 │
        Face Recognition   Voice Recognition
              │                 │
              └────────┬────────┘
                       ↓
               Supabase / PostgreSQL
                       ↓
            Attendance & User Records
```

The public landing page is built separately with **Flask** and deployed on **Vercel**, while the AI application runs on **Streamlit Cloud**.

---

## 🗄️ Database

Main PostgreSQL tables:

```text
teachers
students
subjects
student_subjects
attendance_logs
```

Relationship:

```text
teachers
   │
   └── subjects
          │
          └── student_subjects ── students

attendance_logs
   ├── student_id
   └── subject_id
```

---

## 🔐 Security

- Teacher passwords are hashed using **bcrypt**.
- Supabase credentials are stored using **Streamlit Secrets**.
- Biometric profiles are represented as stored **face and voice embeddings**.

---

## 🌐 Deployment

| Part | Platform |
|---|---|
| 🤖 AI Attendance App | Streamlit Cloud |
| 🌐 Landing Page | Vercel |
| 🗄️ Database | Supabase |

---

## 💡 Why SnapClass?

```text
Manual Attendance
       ↓
      ❌ Slow
      ❌ Repetitive
      ❌ Difficult to manage

        VS

SnapClass
       ↓
   📸 Face AI
   🎙️ Voice AI
   🔳 QR Enrollment
   📊 Digital Records
```

---

## 📌 Project Status

> **Working Prototype / Active Development**

The current system is focused on the core attendance workflow, biometric identification, subject enrollment, dashboards, and cloud deployment.

---

<div align="center">

### 👩‍💻 Built by **Aayesha Singh**

<p>
  <a href="https://github.com/Aayesha2103">GitHub Profile</a>
  ·
  <a href="https://snapclass-main-2ldavrzrff4rtafhtz3xii.streamlit.app/">Try SnapClass</a>
</p>

</div>
