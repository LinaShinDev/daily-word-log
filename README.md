# Daily Word Log 

> **Personal Faith & Daily Meditation Tracking Web Application**  
> A mobile-friendly web application designed to capture daily Scripture verses, personal reflections, and prayer notes—helping users track spiritual growth, manage weekly habits, and explore keyword trends over time.

**[Service Link]** https://daily-word-project.vercel.app/

---

## Background & Motivation
- **The Problem:** Writing quiet time (QT) notes in physical notebooks often leads to forgotten insights and lost momentum. Existing apps often enforce rigid single-entry limits, lack habit tracking, or offer rigid daily devotionals.
- **The Solution:** A flexible, user-driven digital log where users can record **unlimited Scripture entries** per day, attach custom QT sources, manage **weekly faith goals with day-by-day habit tracking (Smart Fallback)**, tag key themes, and leverage **AI-powered tag extraction** for seamless keyword organization.

---

## Tech Stack & Architecture

### Frontend
- **Framework:** React.js (Vite)
- **Styling:** Tailwind CSS (Mobile-first PWA responsive design)
- **State Management & Routing:** React Context API / React Router

### Backend & Database
- **Framework:** Python 3.14 / FastAPI (Uvicorn)
- **Database:** PostgreSQL on Supabase (`uuid` primary keys, Raw SQL with SQLAlchemy `text()` and Upsert patterns)
- **Authentication:** JWT-based persistent secure authentication (`AuthContext`)

### AI / MLOps Features
- **AI Tag Extraction:** Integrated OpenAI (`gpt-4o-mini`) API endpoint (`/api/v1/ai/extract-tags`) with a robust rule-based mock engine fallback.
- **Planned Expansion:** Vector search / RAG for theological book recommendations and weekly AI spiritual reports.

---

## Key Features

1. **Flexible Meditation & Scripture Log**
   - Unlimited daily entry registration with AI-assisted or user-defined custom tag management.
2. **Weekly Goal Tracker & Habit Grid (일~토)**
   - Day-by-day toggle tracking with automated PostgreSQL `ON CONFLICT` Upsert persistence.
   - **Smart Fallback System:** Automatically loads current week goals; if none exist, intelligently fetches the previous week's goal text while resetting habit check states for a fresh start.
3. **AI-Powered Keyword Tagging**
   - Automatically extracts core spiritual keywords and themes from meditation notes.
---

## Database Schema (PostgreSQL DDL)
```mermaid 
erDiagram
	login_user ||--o{ user_detail : references
    category ||--o{ category : references
    category ||--o{ institute : references
	category ||--o{ user_detail : references
	category ||--o{ daily_quotes : references
    user_detail ||--o{ user_qt : references
    user_detail ||--o{ daily_log : references
	user_detail ||--o{ weekly_goals : references
    institute ||--o{ user_qt : references
    daily_log ||--o{ word_entries : references
    user_detail ||--o{ user_tag : references
    content_tag ||--o{ user_tag : references
    word_entries ||--o{ user_tag : references
    user_detail ||--o| user_setting : references
	daily_quotes ||--o{ quote_translation : references
	

	
	login_user {
		UUID login_user_code PK
		VARCHAR(255) email
		VARCHAR(255) enc_pw
		TIMESTAMPTZ insert_dt
		TIMESTAMPTZ update_dt
		TIMESTAMPTZ delete_dt
	}

	user_detail {
		UUID user_detail_code PK
		VARCHAR(255) name
		DATE birth_date
		BigInt denomination_code FK "category.category_code"
		int worship_count
		BOOLEAN is_serving
		BOOLEAN do_qt
		BigInt qt_media FK "category.category_code"
		VARCHAR(255) qt_etc
		TIMESTAMPTZ insert_dt
		TIMESTAMPTZ update_dt
		TIMESTAMPTZ delete_dt
		UUID login_user_code FK
	}

	user_setting {
        BigInt user_setting_code PK
        UUID user_detail_code FK
        BOOLEAN ai_book_recommend_yn
        BOOLEAN qt_check_yn
        BOOLEAN daily_prayer_yn
        BOOLEAN examen_prayer_yn
		VARCHAR(10) language_code
        time qt_notify_time
        time daily_prayer_notify_time
        time examen_prayer_notify_time
        timestamp updated_dt
    }

	category {
		BigInt category_code PK
		VARCHAR(255) category_name
		BigInt parent_category_code FK "category.category_code"
		TIMESTAMPTZ insert_dt
		TIMESTAMPTZ update_dt
		TIMESTAMPTZ delete_dt
	}

	institute {
		BigInt institute_code PK
		VARCHAR(255) institute_name
		TIMESTAMPTZ insert_dt
		TIMESTAMPTZ update_dt
		TIMESTAMPTZ delete_dt
		BigInt institute_type_code FK "category.category_code"
	}

	user_qt {
		BigInt user_qt_code PK
		DATE start_dt
		BigInt institute_code FK
		DATE end_dt
		TIMESTAMPTZ insert_dt
		TIMESTAMPTZ update_dt
		TIMESTAMPTZ delete_dt
		UUID user_detail_code FK
	}

	daily_log {
		BigInt daily_log_code PK
		UUID user_detail_code FK
		Varchar(255) type 
		DATE meditation_date
		BOOLEAN is_completed
		TIMESTAMPTZ insert_dt
		TIMESTAMPTZ update_dt
		TIMESTAMPTZ delete_dt
	}

	word_entries {
		UUID word_entry_code PK
		TEXT bible_verse
		TEXT content
		Varchar(20) category
		TIMESTAMPTZ insert_dt
		TIMESTAMPTZ update_dt
		TIMESTAMPTZ delete_dt
		BigInt daily_log_code FK
		BOOLEAN public_yn
	}

	content_tag {
		BigInt content_tag_code PK
		VARCHAR(255) tag_name
		TIMESTAMPTZ insert_dt
		TIMESTAMPTZ update_dt
		TIMESTAMPTZ delete_dt
	}

	user_tag {
		UUID user_detail_code PK, FK
		BigInt content_tag_code PK, FK
		UUID word_entry_code PK, FK
	}

	daily_quotes {
		BIGINT daily_quote_code PK
	    BIGINT category_code
	    VARCHAR(255) source
		TIMESTAMPTZ insert_dt
		TIMESTAMPTZ update_dt
		TIMESTAMPTZ delete_dt
	}

	quote_translation {
		BIGINT quote_translation_code PK
	    BIGINT daily_quote_code
	    VARCHAR(10) language_code
	    TEXT content
	    TEXT meditation_guide 
	    VARCHAR(255) keyword 
	   cTIMESTAMPTZ insert_dt
		TIMESTAMPTZ update_dt
		TIMESTAMPTZ delete_dt
	}

	weekly_goals {
	    BIGSERIAL weekly_goal_code PK
	    BIGINT user_detail_code 
	    DATE start_date 
	    VARCHAR(255) goal_content
	    BOOLEAN is_sun 
	    BOOLEAN is_mon 
	    BOOLEAN is_tue 
	    BOOLEAN is_wed 
	    BOOLEAN is_thu 
	    BOOLEAN is_fri 
	    BOOLEAN is_sat
		BOOLEAN is_copied
	    
	   	TIMESTAMPTZ insert_dt
		TIMESTAMPTZ update_dt
	}


```
---

## Roadmap & Development Status

### Core Development Phase
- [x] **Phase 1: DB & ERD Design** - Finalized PostgreSQL DDL schema with proper FK constraints, UUIDs, and unique UPSERT constraints.
- [x] **Phase 2: UI/UX Wireframe & Auth** - Set up React/Vite, Tailwind CSS, and JWT-based persistent authentication (`AuthContext`).
- [x] **Phase 3: Core Logging Features** - Implemented daily word entry forms with unlimited entries and AI tag extraction (`gpt-4o-mini`).
- [x] **Phase 4: Weekly Habit Tracker & Smart Fallback** - Day-by-day habit toggling grid with intelligent previous-week goal recovery.
- [ ] **Phase 5: Tag & Category Management** - Display recent tags, filter entries by tag clicks, and localize categories.
- [ ] **Phase 6: Summary Dashboard & Analytics** - Keyword frequency charts and spiritual growth trend visualization.
