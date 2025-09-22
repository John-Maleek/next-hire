# NextHire – Job Application App

A fullstack **Next.js application** that connects recruiters with applicants.  
Recruiters can post jobs and manage applications, while applicants can browse jobs, apply, and track their application status.  

---

## 🚀 Features

### 👤 Applicant
- Create account and manage profile (resume, skills, personal info)
- Browse and search job postings
- Apply to jobs with resume + cover letter
- Track application status (Pending, Shortlisted, Rejected, Hired)

### 🏢 Recruiter
- Create account and manage company profile
- Post, edit, and delete job listings
- View applicants for each posting
- Update application statuses

### 🔐 Shared
- Secure authentication (NextAuth.js)
- Role-based dashboards (Recruiter vs Applicant)
- File uploads for resumes
- Responsive UI (TailwindCSS + shadcn/ui)

---

## 🛠️ Tech Stack

- **Framework:** [Next.js 13+ (App Router)](https://nextjs.org/docs/app)
- **Database & ORM:** PostgreSQL + [Prisma](https://www.prisma.io/)
- **Authentication:** [NextAuth.js](https://next-auth.js.org/)
- **File Storage:** AWS S3 / Cloudinary / UploadThing
- **Styling:** [TailwindCSS](https://tailwindcss.com/) + [shadcn/ui](https://ui.shadcn.com/)
- **State Management:** React Query or Redux Toolkit

---

## 📂 Project Structure


---

## 🗄️ Database Schema (Prisma)

```prisma
model User {
  id          String   @id @default(uuid())
  name        String
  email       String   @unique
  password    String
  role        Role
  jobs        Job[]    @relation("RecruiterJobs")
  applications Application[]
}

model Job {
  id          String   @id @default(uuid())
  title       String
  description String
  location    String
  salaryRange String?
  recruiter   User     @relation("RecruiterJobs", fields: [recruiterId], references: [id])
  recruiterId String
  applications Application[]
}

model Application {
  id          String @id @default(uuid())
  applicant   User   @relation(fields: [applicantId], references: [id])
  applicantId String
  job         Job    @relation(fields: [jobId], references: [id])
  jobId       String
  resumeUrl   String?
  coverLetter String?
  status      ApplicationStatus @default(PENDING)
}

enum Role {
  RECRUITER
  APPLICANT
}

enum ApplicationStatus {
  PENDING
  SHORTLISTED
  REJECTED
  HIRED
}


