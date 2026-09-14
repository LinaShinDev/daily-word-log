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
	user_detail ||--o| user_setting : references

	
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
		VARCHAR(255) donomination_code
		int worship_count
		BOOLEAN is_serving
		BOOLEAN do_qt
		VARCHAR(255) qt_media
		VARCHAR(255) qt_etc
		TIMESTAMPTZ insert_dt
		TIMESTAMPTZ update_dt
		TIMESTAMPTZ delete_dt
		UUID login_user_code
	}

	user_setting {
        bigint user_setting_code
        uuid user_detail_code
        boolean ai_book_recommend_yn
        boolean qt_check_yn
        boolean daily_prayer_yn
        boolean examen_prayer_yn
        time qt_notify_time
        time daily_prayer_notify_time
        time examen_prayer_notify_time
        timestamp updated_dt
    }

	category {
		BigInt category_code
		VARCHAR(255) category_name
		BigInt parent_category_code
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
---

### Roadmap 
[x] Phase 1: DB & ERD Design - Finalized PostgreSQL DDL schema with proper FK constraints.
[ ] Phase 2: UI/UX Wireframe & Auth - Set up React project, Tailwind CSS, and user signup/login flow.
[ ] Phase 3: Core Logging Features - Implement daily word entry forms with unlimited entries and image uploads.
[ ] Phase 4: Calendar & List View - Build daily check-in calendar and timeline feeds.
[ ] Phase 5: Summary Dashboard - Create keyword frequency charts and monthly trend analytics.
[ ] Phase 6: Community Feed (Future) - Optional public sharing (public_yn) for encouragement and community posts.
