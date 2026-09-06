# Daily Word Log 

> **Personal Faith & Daily Meditation Tracking Web Application**  
> A mobile-friendly web application designed to capture daily Scripture verses, personal reflections, and prayer notes—helping users track spiritual growth and keyword trends over time.

---

## Background & Motivation
- **The Problem:** Writing quiet time (QT) notes in physical notebooks often leads to forgotten insights and lost momentum. Existing apps often enforce rigid single-entry limits or fixed daily devotionals.
- **The Solution:** A flexible, user-driven digital log where users can record **unlimited Scripture entries** per day, attach custom QT sources or book covers, tag key themes (e.g., `#Grace`, `#Obedience`), and visualize spiritual growth patterns via a summary dashboard.

---

## 🛠️ Tech Stack & Architecture

### Frontend
- **Framework:** React.js
- **Styling:** Tailwind CSS (Mobile-first responsive design)
- **State Management:** React Context API / Zustand

### Backend & Database
- **Database:** PostgreSQL
- **Authentication:** Supabase Auth / JWT
- **Storage:** Cloud Storage for devotional book cover images & notes

---

## Database Schema (PostgreSQL DDL)
```mermaid 
erDiagram
	login_user ||--o{ user_detail : references
	category ||--o{ category : references
	category ||--o{ institute : references
	user_detail ||--o{ user_qt : references
	institute ||--o{ user_qt : references
	user_qt ||--o{ daily_log : references
	daily_log ||--o{ word_entries : references
	user_detail ||--o{ user_tag : references
	content_tag ||--o{ user_tag : references

	login_user {
		UUID login_user_code
		VARCHAR(255) email
		VARCHAR(255) enc_pw
		TIMESTAMPTZ insert_dt
		TIMESTAMPTZ update_dt
		TIMESTAMPTZ delete_dt
	}

	user_detail {
		UUID user_detail_code
		VARCHAR(255) name
		DATE birth_date
		TIMESTAMPTZ insert_dt
		TIMESTAMPTZ update_dt
		TIMESTAMPTZ delete_dt
		UUID login_user_code
	}

	category {
		VARCHAR(255) category_code
		VARCHAR(255) category_name
		VARCHAR(255) parent_category_code
		TIMESTAMPTZ insert_dt
		TIMESTAMPTZ update_dt
		TIMESTAMPTZ delete_dt
	}

	institute {
		VARCHAR(255) institute_code
		VARCHAR(255) institute_name
		TIMESTAMPTZ insert_dt
		TIMESTAMPTZ update_dt
		TIMESTAMPTZ delete_dt
		VARCHAR(255) institute_type_code
	}

	user_qt {
		VARCHAR(255) user_qt_code
		DATE start_dt
		VARCHAR(255) institute_code
		DATE end_dt
		TIMESTAMPTZ insert_dt
		TIMESTAMPTZ update_dt
		TIMESTAMPTZ delete_dt
		UUID user_detail_code
	}

	daily_log {
		VARCHAR(255) daily_log_code
		VARCHAR(255) user_qt_code
		DATE meditation_date
		BOOLEAN is_completed
		TIMESTAMPTZ insert_dt
		TIMESTAMPTZ update_dt
		TIMESTAMPTZ delete_dt
	}

	word_entries {
		VARCHAR(255) word_entry_code
		TEXT bible_verse
		TEXT content
		TIMESTAMPTZ insert_dt
		TIMESTAMPTZ update_dt
		TIMESTAMPTZ delete_dt
		VARCHAR(255) daily_log_code
		BOOLEAN public_yn
	}

	content_tag {
		VARCHAR(255) content_tag_code
		VARCHAR(255) tag_name
		TIMESTAMPTZ insert_dt
		TIMESTAMPTZ update_dt
		TIMESTAMPTZ delete_dt
	}

	user_tag {
		UUID user_detail_code
		VARCHAR(255) content_tag_code
	}
```
