# 📸 SnapClass — AI-Powered Attendance System

<p align="center">

  <img src="https://i.ibb.co/YTYGn5qV/logo.png" alt="SnapClass Logo" width="120">

  <h2 align="center">SnapClass</h2>

  <p align="center">
    AI-powered classroom attendance using Face Recognition and Voice Recognition.
  </p>

  <p align="center">
    <a href="https://sc-landing-page-5zcx0j33w-aayesha-singh.vercel.app/">
    🌐 Landing Page
    </a>
    ·
    <a href="https://snapclass-main-2ldavrzrff4rtafhtz3xii.streamlit.app/">
      🚀 Live Application
    </a>
    ·
    <a href="https://github.com/Aayesha2103/snapclass-main">
      💻 GitHub
    </a>
  </p>

</p>

---

## 🧠 What is SnapClass?

**SnapClass** is an AI-powered classroom attendance management system designed to automate the traditional attendance process.

Instead of manually calling names or maintaining attendance sheets, teachers can use:

- 📸 **Face Recognition** for classroom attendance
- 🎙️ **Voice Recognition** for sequential voice-based attendance
- 🔳 **QR / Subject Codes** for quick student enrollment
- 📊 **Attendance Records and Analytics**
- 👨‍🏫 **Teacher Dashboard**
- 👨‍🎓 **Student Dashboard**

Students can register themselves, enroll in subjects, and view their attendance information, while teachers can create subjects, manage classes, take attendance, and review historical attendance records.

---

## 🎯 Problem Statement

Traditional classroom attendance can be:

- Time-consuming
- Manual
- Error-prone
- Difficult to manage for large classes
- Difficult to maintain digitally over time

SnapClass aims to reduce this manual work by using **biometric identification and automated attendance processing**.

---

# ✨ Features

## 👨‍🏫 Teacher Portal

### 🔐 Teacher Authentication
- Teacher registration
- Teacher login
- Password hashing using `bcrypt`

### 📚 Subject Management
- Create subjects
- Add subject code
- Add section information
- Manage teacher-owned subjects

### 🔗 Subject Sharing
- Share subject codes
- Generate QR codes for class joining
- Students can quickly enroll in subjects

### 📸 AI Face Attendance
- Add classroom photos
- Detect multiple faces
- Generate face embeddings
- Predict student identities
- Compare recognition confidence
- Mark students as Present / Absent

### 🎙️ AI Voice Attendance
- Record classroom audio
- Segment audio into speech sections
- Generate speaker embeddings
- Compare voices with enrolled profiles
- Identify students using voice similarity

### 📊 Attendance Records
- View historical attendance
- Group attendance by subject and session
- Display present/total student counts
- Track attendance over time

---

# 👨‍🎓 Student Portal

### 📸 Face ID Login
Students can log into SnapClass by positioning their face in front of the camera.

### 🆕 New Student Registration
If a face is not recognized:

1. The student enters their name.
2. Their face embedding is generated.
3. The face embedding is stored.
4. The student can optionally enroll their voice.

This creates a biometric profile for future recognition.

### 📚 Subject Enrollment
Students can:

- Enter a subject code
- Join a class
- Prevent duplicate enrollment
- Unenroll from subjects

### 📊 Student Dashboard
Students can view:

- Enrolled subjects
- Subject codes
- Sections
- Attendance statistics

---

# 🧠 AI Architecture

SnapClass uses two biometric pipelines.

## 📸 Face Recognition Pipeline

```text
Classroom Image
      ↓
Face Detection
      ↓
Facial Landmark Detection
      ↓
128-D Face Embedding
      ↓
SVC Classifier
      ↓
Student ID Prediction
      ↓
Embedding Distance Check
      ↓
Attendance
Technologies Used
dlib
face_recognition_models
NumPy
scikit-learn
Face Detection

Dlib's frontal face detector is used to identify faces in classroom images.

Facial Landmarks

A Dlib shape predictor is used to locate facial landmarks.

Face Embeddings

The Dlib face-recognition model converts a detected face into a 128-dimensional numerical embedding.

Conceptually:

Face Image
    ↓
[0.14, -0.32, 0.76, ... , 0.08]

The embedding represents the facial characteristics needed for identity comparison.

Classification

The stored embeddings are used to train a:

SVC(
    kernel="linear",
    probability=True,
    class_weight="balanced"
)

The classifier maps:

Face Embedding → Student ID

An additional embedding-distance check is performed using:

np.linalg.norm()

to reject weak matches.

🎙️ Voice Recognition Pipeline
Classroom Audio
      ↓
Audio Loading
      ↓
Speech Segmentation
      ↓
Voice Embedding
      ↓
Similarity Comparison
      ↓
Student Identification
      ↓
Attendance
Technologies Used
Resemblyzer
librosa
NumPy
Audio Processing

Audio is loaded at:

16 kHz

using Librosa.

Speech regions are identified with:

librosa.effects.split()

Short segments are ignored.

Voice Embeddings

Resemblyzer converts speech into a speaker embedding.

The new embedding is compared against stored student voice embeddings.

A similarity threshold is used to decide whether a voice is considered a match.

🗄️ Database Architecture

SnapClass uses:

Supabase
PostgreSQL

The main tables are:

teachers
students
subjects
student_subjects
attendance_logs
Database Relationship
teachers
    │
    │ teacher_id
    ▼
subjects
    │
    │ subject_id
    ▼
student_subjects
    │
    │ student_id
    ▼
students

Attendance records connect the student and subject:

attendance_logs
├── student_id
└── subject_id
👨‍🏫 teachers

Stores teacher authentication and profile information.

teacher_id
username
password
name

Passwords are hashed using bcrypt.

👨‍🎓 students

Stores student information and biometric embeddings.

student_id
name
face_embedding
voice_embedding

Face and voice embeddings are stored in PostgreSQL using JSONB.

📚 subjects

Stores:

subject_id
subject_code
name
section
teacher_id
🔗 student_subjects

This is the student-subject relationship table.

student_id
subject_id

It allows:

one student → multiple subjects
one subject → multiple students

The combination of subject_id and student_id is unique.

📝 attendance_logs

Stores attendance history:

student_id
subject_id
timestamp
is_present
🏗️ Project Architecture

The project is organized into separate layers rather than keeping everything in one file.

snapclass/
│
├── app.py
│
├── src/
│   │
│   ├── components/
│   │   ├── header.py
│   │   ├── footer.py
│   │   ├── subject_card.py
│   │   ├── dialog_create_subject.py
│   │   ├── dialog_share_subject.py
│   │   ├── dialog_enroll.py
│   │   ├── dialog_voice_attendance.py
│   │   └── ...
│   │
│   ├── database/
│   │   ├── config.py
│   │   └── db.py
│   │
│   ├── pipelines/
│   │   ├── face_pipeline.py
│   │   └── voice_pipeline.py
│   │
│   ├── screens/
│   │   ├── teacher_screen.py
│   │   └── student_screen.py
│   │
│   └── ui/
│       └── base_layout.py
│
├── .streamlit/
│   └── secrets.toml
│
├── requirements.txt
└── README.md
🧩 Technology Stack
Category	Technology
Main Language	Python
Application UI	Streamlit
Landing Page	Flask
Frontend Styling	HTML, CSS
Database	Supabase / PostgreSQL
Authentication	bcrypt
Face Detection	dlib
Face Recognition	dlib / face_recognition_models
ML Classifier	scikit-learn SVC
Numerical Processing	NumPy
Audio Processing	Librosa
Voice Embeddings	Resemblyzer
Image Processing	Pillow
Data Analysis	Pandas
QR Generation	Segno
Version Control	Git / GitHub
Application Deployment	Streamlit Cloud
Landing Page Deployment	Vercel
⚙️ Important Python Libraries
Streamlit

Used for:

User interface
Dashboards
Buttons
Forms
Camera input
Audio input
Dialogs
Session state
Dataframes
NumPy

Used for:

Face embeddings
Voice embeddings
Vector operations
Distance calculations
Similarity calculations
Pandas

Used for:

Attendance DataFrames
Grouping attendance
Attendance summaries
Sorting and displaying attendance records
Dlib

Used for:

Face detection
Facial landmarks
Face descriptors
face_recognition_models

Provides the pretrained face-recognition models used by Dlib.

scikit-learn

Used for the SVC classifier that maps face embeddings to student identities.

Resemblyzer

Used to generate speaker embeddings from student voices.

Librosa

Used for:

Loading audio
Resampling audio
Audio segmentation
Speech preprocessing
Pillow

Used to open and convert captured images before passing them into the face pipeline.

bcrypt

Used to hash teacher passwords before storing them.

Segno

Used to generate QR codes for subject enrollment.

🔐 Authentication & Security

Teacher passwords are hashed instead of being stored as plaintext.

The flow is:

Password
   ↓
bcrypt.hashpw()
   ↓
Password Hash
   ↓
Supabase

During login:

Entered Password
       ↓
bcrypt.checkpw()
       ↓
Valid / Invalid

Supabase credentials are stored using Streamlit secrets and should not be committed to GitHub.

🚀 How SnapClass Works End-to-End
Teacher Flow
Teacher
   ↓
Login / Register
   ↓
Teacher Dashboard
   ↓
Create Subject
   ↓
Share Subject Code / QR
   ↓
Students Enroll
   ↓
Take Attendance
   ├── Face Attendance
   └── Voice Attendance
   ↓
Attendance Results
   ↓
Attendance Records
Student Flow
Student
   ↓
Face ID Login
   ↓
Face Recognized?
   ├── YES → Dashboard
   │
   └── NO
        ↓
      Register
        ↓
Face Embedding
        +
Optional Voice Embedding
        ↓
Student Profile
        ↓
Enroll in Subjects
        ↓
Student Dashboard
📸 Face Attendance — Detailed Flow
Teacher selects a subject.
Teacher adds classroom photographs.
Each image is converted into a NumPy array.
Dlib detects faces.
Facial landmarks are extracted.
A 128-dimensional face descriptor is generated.
The SVC classifier predicts the student identity.
An embedding-distance check is performed.
Recognized student IDs are collected.
The system fetches students enrolled in the selected subject.
Recognized IDs are compared with enrolled student IDs.
Students are marked Present or Absent.
Attendance data is prepared for storage.
🎙️ Voice Attendance — Detailed Flow
Teacher selects a subject.
Classroom audio is recorded.
Audio is loaded at 16 kHz.
Librosa detects usable speech segments.
Very short segments are ignored.
Resemblyzer creates voice embeddings.
Each embedding is compared with stored student embeddings.
The highest similarity is selected.
A threshold determines whether the match is accepted.
Attendance data is generated.
📚 Subject Enrollment Flow
Teacher
   ↓
Creates Subject
   ↓
Subject Code
   ↓
Student receives Code
   ↓
Student enters Code
   ↓
Find Subject
   ↓
Check Existing Enrollment
   ↓
Insert into student_subjects

QR sharing provides a faster version of the same workflow.

⚡ Performance Optimization

AI models are expensive to load repeatedly.

SnapClass uses:

@st.cache_resource

for expensive model initialization.

For example:

@st.cache_resource
def load_dlib_models():
    ...

and:

@st.cache_resource
def load_voice_encoder():
    ...

This prevents model initialization on every Streamlit rerun.

🌐 Deployment Architecture

SnapClass uses two deployment layers.

                    Internet
                       │
            ┌──────────┴──────────┐
            │                     │
         Vercel             Streamlit Cloud
            │                     │
       Flask Landing         AI Application
          Page                │
                              │
                           Supabase
                              │
                         PostgreSQL
Vercel

Used for the public Flask landing page.

Streamlit Cloud

Used for the actual AI attendance application.

Supabase

Used as the cloud database backend.

🖥️ Landing Page

The landing page is built separately using:

Flask
Jinja templates
HTML
CSS
Static images

It provides:

Project introduction
Features
Teacher journey
Student journey
Technology stack
Link to the live application

The landing page redirects users to the main Streamlit application.

📂 Why the Project is Modular

Instead of putting everything into app.py, SnapClass separates responsibilities.

components/

Reusable UI elements and dialogs.

screens/

Complete teacher and student screens.

database/

Supabase connection and database operations.

pipelines/

AI processing pipelines.

ui/

Global styling and layout.

This makes the project easier to:

maintain
debug
extend
explain
test
🧪 Current Limitations

SnapClass is currently a strong working prototype, but there are areas that would need more validation before production deployment.

Face Recognition

Recognition thresholds should be validated using a proper dataset.

Metrics such as:

Accuracy
Precision
Recall
False Acceptance Rate
False Rejection Rate

should be measured.

Voice Recognition

Voice similarity thresholds also need real-world validation across:

different microphones
background noise
different distances
different speaking styles
Biometric Privacy

Production deployment should include:

explicit consent
stronger access control
encryption
biometric data retention policies
deletion mechanisms
privacy documentation
🔮 Future Improvements

Possible future improvements include:

Real-time camera attendance
Better multi-face tracking
Improved voice diarization
Attendance trend visualization
Teacher analytics dashboard
Email / notification system
Stronger authentication
Role-based authorization
Better biometric security
Automated attendance reports
Export to Excel/PDF
Mobile-first UI improvements
Production-grade biometric validation
🧑‍💻 Running the Project Locally
1. Clone the repository
git clone https://github.com/Aayesha2103/snapclass-main.git
cd snapclass-main
2. Create a virtual environment
Windows
python -m venv venv

Activate it:

.\venv\Scripts\Activate.ps1
3. Install dependencies
python -m pip install -r requirements.txt
4. Configure Streamlit secrets

Create:

.streamlit/secrets.toml

Example:

SUPABASE_URL = "YOUR_SUPABASE_URL"
SUPABASE_KEY = "YOUR_SUPABASE_KEY"

Do not commit this file to GitHub.

5. Run the application
python -m streamlit run app.py
🔑 Environment Variables / Secrets

The application requires Supabase credentials.

SUPABASE_URL
SUPABASE_KEY

Keep these credentials private.

For Streamlit Cloud, add them through the application's Secrets settings rather than committing them to the repository.

📌 Project Highlights

✅ AI-powered attendance
✅ Face recognition
✅ Voice recognition
✅ Student registration
✅ Teacher authentication
✅ Subject management
✅ QR-based enrollment
✅ Supabase/PostgreSQL database
✅ Attendance analytics
✅ Modular architecture
✅ Streamlit AI application
✅ Flask landing page
✅ Cloud deployment

🎓 What I Learned Building SnapClass

This project involved practical work across multiple areas:

Python application development
Streamlit UI development
Database design
Supabase/PostgreSQL
Authentication
Face recognition
Machine learning classification
Voice embeddings
Audio processing
Vector similarity
State management
Modular software architecture
Git/GitHub
Cloud deployment
Debugging deployment issues
Responsive HTML/CSS design
💡 Why SnapClass?

Traditional attendance asks:

"Who's present?"

SnapClass asks:

"Can AI identify who's present?"

The goal is to make classroom attendance:

Faster → Smarter → More Automated

⭐ Author

Aayesha Singh

GitHub:
https://github.com/Aayesha2103

📜 Project Status

Status: Active Development / Working Prototype
