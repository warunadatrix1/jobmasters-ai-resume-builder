# Discovery Phase Findings - JobMasters AI Resume Builder

**Date**: 2026-05-13  
**Inspector**: warunadatrix1  
**Database**: jobmasters.lk (XAMPP local)  
**phpMyAdmin URL**: http://localhost/phpmyadmin

---

## 1. Database Schema Discovery

### ✅ Table Prefix
**Confirmed**: `wp_` (standard WordPress prefix)

### ✅ Candidate Post Type
**Confirmed**: `post_type = 'candidate'`

Example records:
```
ID=781,  title="HASITHA K. WAIDYARATNE",        post_author=120, date=2024-05-28
ID=827,  title="D.N.DISSANAYAKE",               post_author=203, date=2024-05-28
ID=829,  title="ASANGA WIJERATNE",              post_author=204, date=2024-05-28
ID=936,  title="Kalana Bamunuarachchi",         post_author=254, date=2024-05-28
ID=1023, title="M.D Mahesh Senadeera",          post_author=297, date=2024-05-28
```

**Relationship**: `wp_users.ID` → `wp_posts.post_author` (for candidate posts)

---

## 2. Candidate Meta Keys (wp_postmeta)

### Core Resume Data Fields
These are stored as `wp_postmeta` on the candidate post with `jobsearch_field_*` prefix (WP JobSearch plugin standard):

#### Contact Information
- `jobsearch_field_user_email` - Candidate email ✅
- `jobsearch_field_user_phone` - Phone number ✅
- `jobsearch_field_user_justphone` - Formatted phone ✅
- `jobsearch_field_user_dial_code` - Country dial code (e.g., "+94") ✅
- `jobsearch_field_contry_iso_code` - Country ISO code (e.g., "lk") ✅
- `jobsearch_field_location_address` - Full address ✅
- `jobsearch_field_location_postalcode` - Postal code ✅
- `jobsearch_field_location_lat` / `lng` - GPS coordinates
- Social URLs:
  - `jobsearch_field_user_linkedin_url`
  - `jobsearch_field_user_twitter_url`
  - `jobsearch_field_user_facebook_url`
  - `jobsearch_field_user_dribbble_url`
  - `jobsearch_field_user_google_plus_url`

#### Profile Data
- `jobsearch_field_candidate_jobtitle` - Current/desired job title
- `jobsearch_field_candidate_salary` - Expected salary ✅
- `jobsearch_field_candidate_salary_type` - Salary frequency (monthly/annual)
- `jobsearch_field_candidate_approved` - Profile approval status ✅
- `jobsearch_field_user_public_pview` - Public profile visibility
- `jobsearch_field_user_avatar_url` - Profile image URL
- `jobsearch_user_cover_imge` - Cover image URL

#### Professional Data
- `jobsearch_field_candidate_experience_inyears` - Years of experience
- `jobsearch_field_user_cv_attachments` - Uploaded CV file(s)
- `jobsearch_field_resume_cover_letter` - Cover letter

#### Experience (Repeating Fields - Serialized Arrays)
- `jobsearch_field_experience_title` - Job title
- `jobsearch_field_experience_company` - Company name
- `jobsearch_field_experience_description` - Job description
- `jobsearch_field_experience_start_date` - Start date
- `jobsearch_field_experience_end_date` - End date
- `jobsearch_field_experience_date_prsnt` - Currently working there?

#### Education (Repeating Fields - Serialized Arrays)
- `jobsearch_field_education_title` - Degree/Course name
- `jobsearch_field_education_academy` - School/University name
- `jobsearch_field_education_description` - Additional notes
- `jobsearch_field_education_start_date` - Start date
- `jobsearch_field_education_end_date` - End date
- `jobsearch_field_education_date_prsnt` - Currently studying?

#### Skills (Repeating Fields - Serialized Arrays)
- `jobsearch_field_skill_title` - Skill name
- `jobsearch_field_skill_percentage` - Proficiency level (0-100)
- `jobsearch_field_skill_desc` - Skill description

#### Languages (Repeating Fields)
- `jobsearch_field_lang_title` - Language name
- `jobsearch_field_lang_level` - Level (Basic/Intermediate/Advanced)
- `jobsearch_field_lang_percentage` - Proficiency percentage

#### Certifications & Awards (Repeating Fields)
- `jobsearch_field_award_title` - Certification/Award name
- `jobsearch_field_award_description` - Description
- `jobsearch_field_award_year` - Year obtained

#### Portfolio (Repeating Fields)
- `jobsearch_field_portfolio_title` - Project title
- `jobsearch_field_portfolio_url` - Project URL
- `jobsearch_field_portfolio_image` - Project image
- `jobsearch_field_portfolio_vurl` - Video URL (optional)

#### Additional Profile Fields
- `professional-level` - Professional level (e.g., "Intern") ✅
- `industry-specific-skills` - Industry skills
- `certifications-licenses-*` - Certification data
- `professional-development-training-*` - Training records
- `conferences-workshops-*` - Conferences attended
- `publications` - Publications/research
- `volunteer-work` - Volunteer experience
- `military-service` - Military background
- `media-appearances-interviews` - Media mentions
- `professional-affiliations` - Professional memberships
- `marital-status` - Marital status
- `willingness-to-travel` - Travel willingness
- `relocation-preferences` - Relocation info
- `Age` / `age7529258` - Age (stored in multiple formats, unclear why)
- `gender` - Gender
- `citzenship` - Citizenship
- `academic-level` - Academic qualification level

### Non-Resume Fields (Skip These)
- `_aioseo_*` - SEO plugin metadata (ignore)
- `_edit_last`, `_edit_lock` - WordPress core (ignore)
- `_wpb_vc_editor_type` - Visual Composer (ignore)
- `_wp_*` - WordPress internals (ignore)
- `_ajax_*`, `_nonce*`, `_wp_http_referer` - AJAX/forms (ignore)
- Various form submission temp fields - (ignore)

---

## 3. User Meta Keys (wp_usermeta)

**Candidate profile data ALSO stored in usermeta**:

```
nickname                    - Username/nickname
first_name                  - First name (may be empty)
last_name                   - Last name (may be empty)
description                 - Bio/description
jobsearch_employer_id       - Related employer ID (if any)
wp_capabilities            - WordPress user role
```

**Key finding**: Most data is stored in `wp_postmeta` (candidate post), NOT in `wp_usermeta`. The usermeta is minimal.

---

## 4. Sample Candidate Record Analysis

**Test record**: `dulinahansaja62@gmail.com`

```
User ID:      18754
Login:        Kali
Email:        dulinahansaja62@gmail.com
Display Name: Dulina Palapathwala
Registered:   2026-02-02 17:20:38

Candidate Post:
ID:           99412
Title:        Dulina Palapathwala
Created:      2026-02-02 22:50:38
```

**Meta values found**:
```
Full Name:                    Dulina Palapathwala (from display_name & post_title)
Email:                        dulinahansaja62@gmail.com (confirmed in usermeta)
Phone:                        77 495 1601 (jobsearch_field_user_phone)
Country Code:                 +94 (dial code)
Country ISO:                  lk (jobsearch_field_contry_iso_code)
Professional Level:           Intern
Profile Visibility:           yes
Location Country:             srilanka
Location Province:            norwest (North Western)
Location District:            kurunegala
Postal Code:                  60000
Full Address:                 North Western Province, Sri Lanka, 60000
GPS Lat/Lng:                  37.090240, -95.712891
Overall Skills Percentage:    66%
Candidate Approved:           yes
Registration Data:            2026-02-02 17:20:38
```

---

## 5. Data Structure Patterns

### ✅ Repeating Fields Pattern (WP JobSearch)

Fields like experience, education, skills, languages are stored as **serialized arrays** in meta values.

Example structure (serialized PHP array):
```php
// In wp_postmeta:
meta_key = 'jobsearch_field_experience_title'
meta_value = ['Software Developer', 'Senior Developer'] // or individual rows per meta_key

// OR each item might have a numeric suffix:
meta_key = 'jobsearch_field_experience_title_1'
meta_value = 'Software Developer'
meta_key = 'jobsearch_field_experience_title_2'
meta_value = 'Senior Developer'
```

**ACTION**: Must inspect actual database to confirm serialization pattern for repeating fields.

### ✅ Date Format
- `jobsearch_field_experience_start_date` - Format: (likely YYYY-MM-DD, must verify)
- `jobsearch_field_experience_end_date` - Same
- `jobsearch_field_education_start_date` - Same
- `jobsearch_field_education_end_date` - Same

---

## 6. Careerfy Framework Integration Points

### Likely Careerfy Meta Keys (Not yet confirmed in sample)
- `careerfy_field_candidate_post_detail_style` - Template styling (found in results)
- Any `_careerfy_*` prefix keys (if Careerfy theme customizations exist)

**ACTION**: Search `wp_postmeta` for `careerfy_` or `_careerfy_` pattern to find Careerfy-specific fields.

### Careerfy Dashboard Hook Points
- **Location**: Check theme files in `/wp-content/themes/careerfy/` or child theme
- **Hook candidates**:
  - `careerfy_candidate_dashboard_menu` (hypothetical)
  - `careerfy_dashboard_content_bottom` (hypothetical)
  - Theme template: `template-candidate-dashboard.php` or similar
- **ACTION**: Search theme files for `do_action()` and `apply_filters()` calls in dashboard area

---

## 7. WP JobSearch Plugin Integration

### ✅ Confirmed WP JobSearch Meta Keys
All `jobsearch_field_*` and `jobsearch_*` keys are WP JobSearch plugin standard.

**Plugin handles**:
- Candidate profiles (CPT)
- Profile fields & validation
- Job applications
- Favorites/follows
- Notifications
- Chat/messaging
- Candidate search/filtering

**ACTION**: Inspect `/wp-content/plugins/wp-jobsearch/` for:
- Field definitions
- Meta key naming conventions
- Any custom hooks for dashboard integration
- Existing resume/CV handling

---

## 8. Resume/CV Storage

### Current Implementation
- **CV Upload Field**: `jobsearch_field_user_cv_attachments` - Stores uploaded CV file(s)
- **Cover Letter**: `jobsearch_field_resume_cover_letter` - Stores text/document
- **Existing Generated Resumes**: None found in sample (field may not exist yet)

**ACTION**: Search database for existing resume storage:
```sql
SELECT DISTINCT pm.meta_key
FROM wp_postmeta pm
WHERE pm.meta_key LIKE '%resume%' OR pm.meta_key LIKE '%cv%'
ORDER BY pm.meta_key;
```

---

## 9. Assumptions Verified ✅

- [x] Table prefix: `wp_` confirmed
- [x] Candidate post type: `post_type = 'candidate'` confirmed
- [x] User ↔ Candidate relationship: `wp_users.ID` → `wp_posts.post_author` confirmed
- [x] Meta storage: All candidate data in `wp_postmeta` on candidate post confirmed
- [x] WP JobSearch integration: All `jobsearch_field_*` meta keys confirmed
- [x] User email lookup: Reliable via `wp_users.user_email` confirmed
- [ ] Repeating field serialization: Pattern NOT YET VERIFIED (need SQL inspection)
- [ ] Careerfy custom meta: Pattern NOT YET VERIFIED (search `careerfy_` prefix)
- [ ] Existing resume/AI storage: None found in sample

---

## 10. Required SQL Queries for Next Phase

### Query: Get All Data for One Candidate (Template)
```sql
SELECT
    u.ID AS user_id,
    u.user_login,
    u.user_email,
    u.display_name,
    u.user_registered,
    p.ID AS post_id,
    p.post_title,
    p.post_content,
    p.post_date,
    pm.meta_key,
    pm.meta_value
FROM wp_users u
LEFT JOIN wp_posts p ON p.post_author = u.ID AND p.post_type = 'candidate'
LEFT JOIN wp_postmeta pm ON pm.post_id = p.ID
WHERE u.user_email = %s
ORDER BY pm.meta_key ASC;
```

### Query: Get Specific Candidate Fields (Optimized)
```sql
SELECT DISTINCT pm.meta_key, pm.meta_value
FROM wp_postmeta pm
JOIN wp_posts p ON pm.post_id = p.ID
JOIN wp_users u ON p.post_author = u.ID
WHERE u.user_email = %s AND p.post_type = 'candidate'
AND pm.meta_key LIKE 'jobsearch_field_%'
ORDER BY pm.meta_key;
```

---

## 11. Data Mapping Registry (COMPLETE)

### Full Field → Meta Key Mapping

| **Resume Field** | **Primary Meta Key** | **Fallback Keys** | **Source** | **Type** |
|---|---|---|---|---|
| **Full Name** | `wp_users.display_name` | `wp_posts.post_title`, `member_display_name` | WordPress/WP JobSearch | text |
| **Email** | `jobsearch_field_user_email` | `wp_users.user_email` | WP JobSearch/WordPress | email |
| **Phone** | `jobsearch_field_user_phone` | `jobsearch_field_user_justphone` | WP JobSearch | tel |
| **Country Code** | `jobsearch_field_user_dial_code` | `dial_code` | WP JobSearch | tel |
| **Country ISO** | `jobsearch_field_contry_iso_code` | `contry_iso_code` | WP JobSearch | select |
| **Address** | `jobsearch_field_location_address` | – | WP JobSearch | textarea |
| **Postal Code** | `jobsearch_field_location_postalcode` | `location_location3` | WP JobSearch | text |
| **Province/State** | `jobsearch_field_location_location2` | – | WP JobSearch | select |
| **District/City** | `jobsearch_field_location_location3` | – | WP JobSearch | select |
| **Country** | `jobsearch_field_location_location1` | – | WP JobSearch | select |
| **LinkedIn URL** | `jobsearch_field_user_linkedin_url` | `cand_user_linkedin_url` | WP JobSearch | url |
| **Twitter URL** | `jobsearch_field_user_twitter_url` | `cand_user_twitter_url` | WP JobSearch | url |
| **Facebook URL** | `jobsearch_field_user_facebook_url` | `cand_user_facebook_url` | WP JobSearch | url |
| **Dribbble URL** | `jobsearch_field_user_dribbble_url` | `cand_user_dribbble_url` | WP JobSearch | url |
| **Profile Image** | `jobsearch_user_avatar_url` | – | WP JobSearch | url |
| **Cover Image** | `jobsearch_user_cover_imge` | – | WP JobSearch | url |
| **Job Title** | `jobsearch_field_candidate_jobtitle` | `job-title` | WP JobSearch | text |
| **Years Experience** | `jobsearch_field_candidate_experience_inyears` | – | WP JobSearch | number |
| **Professional Level** | `professional-level` | – | Custom | select |
| **Expected Salary** | `jobsearch_field_candidate_salary` | `candidate_salary`, `salary` | WP JobSearch | number |
| **Salary Type** | `jobsearch_field_candidate_salary_type` | `candidate_salary_type` | WP JobSearch | select |
| **Profile Approved** | `jobsearch_field_candidate_approved` | – | WP JobSearch | boolean |
| **Skills Overall %** | `overall_skills_percentage` | – | WP JobSearch | number |
| **Work Experience** | `jobsearch_field_experience_*` | See below | WP JobSearch | repeater |
| **Education** | `jobsearch_field_education_*` | See below | WP JobSearch | repeater |
| **Skills** | `jobsearch_field_skill_*` | See below | WP JobSearch | repeater |
| **Languages** | `jobsearch_field_lang_*` | See below | WP JobSearch | repeater |
| **Certifications** | `jobsearch_field_award_*` | `certifications-licenses-*` | WP JobSearch | repeater |
| **Portfolio** | `jobsearch_field_portfolio_*` | See below | WP JobSearch | repeater |
| **CV/Resume File** | `jobsearch_field_user_cv_attachments` | `candidate_cv_file` | WP JobSearch | file |
| **Cover Letter** | `jobsearch_field_resume_cover_letter` | `candidate_cover_letter_file` | WP JobSearch | textarea |
| **Professional Summary** | `user_bio` | `description` | WordPress/Custom | textarea |

#### Repeating Fields Detail

**Work Experience** (Array of objects):
```
jobsearch_field_experience_title
jobsearch_field_experience_company
jobsearch_field_experience_description
jobsearch_field_experience_start_date
jobsearch_field_experience_end_date
jobsearch_field_experience_date_prsnt (boolean: "on" = currently employed)
```

**Education** (Array of objects):
```
jobsearch_field_education_title
jobsearch_field_education_academy
jobsearch_field_education_description
jobsearch_field_education_start_date
jobsearch_field_education_end_date
jobsearch_field_education_date_prsnt (boolean: "on" = currently studying)
```

**Skills** (Array of objects):
```
jobsearch_field_skill_title
jobsearch_field_skill_percentage (0-100)
jobsearch_field_skill_desc
```

**Languages** (Array of objects):
```
jobsearch_field_lang_title
jobsearch_field_lang_level (Basic/Intermediate/Advanced)
jobsearch_field_lang_percentage (0-100)
```

**Certifications/Awards** (Array of objects):
```
jobsearch_field_award_title
jobsearch_field_award_description
jobsearch_field_award_year
```

**Portfolio** (Array of objects):
```
jobsearch_field_portfolio_title
jobsearch_field_portfolio_url
jobsearch_field_portfolio_image
jobsearch_field_portfolio_vurl (video URL, optional)
```

---

## 12. Integration Plan

### Dashboard Button Integration
**Location to identify**:
- Search `/wp-content/themes/careerfy/` for dashboard template
- Look for hook: `do_action('careerfy_dashboard_*')` or similar
- Candidate dashboard URL pattern: `/candidate/` or `/dashboard/`

**Integration method**:
- Use `careerfy_*` hook if available
- OR override template via plugin template directory (`/plugin/templates/careerfy-overrides/`)
- OR add button via custom hook in candidate post type

**ACTION**: Inspect theme files to find exact hook name.

### Data Fetch Points
1. Get logged-in user: `wp_get_current_user()` → `user_email`
2. Get candidate post: Query `wp_posts` where `post_author = user_id AND post_type = 'candidate'`
3. Get all postmeta: Query `wp_postmeta` where `post_id = candidate_post_id`
4. Merge and normalize data into unified array

### Storage Plan
**Option A** (Recommended):
- Create custom post type: `jm_airb_resume` (AI Resume Builder)
- Store generated resume HTML/text in `post_content`
- Store metadata: template used, model, prompt version, etc.
- Link to candidate: `post_parent = candidate_post_id`

**Option B** (Alternative):
- Store generated resume in `wp_postmeta` on candidate post
- Meta key: `jm_airb_generated_resume_v{version}`
- Simple but limits version history

**Recommendation**: Use Option A (custom CPT) for scalability.

---

## 13. Known Unknowns (Still Need to Verify)

- [ ] **Repeating field serialization**: Are experience/education stored as serialized arrays or individual meta entries?
  - Need: Run query to inspect actual meta_value for `jobsearch_field_experience_title`
  
- [ ] **Careerfy custom meta**: Are there `careerfy_*` or `_careerfy_*` fields beyond what WP JobSearch provides?
  - Need: Run `SELECT DISTINCT meta_key FROM wp_postmeta WHERE meta_key LIKE '%careerfy%'`

- [ ] **Dashboard template location**: Where exactly is the candidate dashboard template?
  - Need: Search theme files for dashboard rendering

- [ ] **Existing hooks**: What hooks does Careerfy/WP JobSearch expose for dashboard integration?
  - Need: Grep theme/plugin for `do_action()` calls

- [ ] **PDF requirement**: What PDF library is available on SiteGround? (mPDF, TCPDF, etc.)
  - Need: Check SiteGround hosting constraints

---

## 14. Next Actions

### Immediate (Before Phase 2):
1. [x] ✅ Database schema documented
2. [x] ✅ Meta key mapping complete
3. [ ] Verify repeating field serialization (run SQL)
4. [ ] Find dashboard template file
5. [ ] Identify Careerfy hooks
6. [ ] Test OpenAI API connectivity from XAMPP

### Phase 2 Preparation:
- Create plugin bootstrap file: `jobmasters-ai-resume-builder.php`
- Create admin menu structure
- Register Settings API options
- Build Settings page UI

---

## 15. SQL Queries to Run Next

```sql
-- Find repeating field pattern
SELECT pm.meta_key, COUNT(*) as count, pm.meta_value
FROM wp_postmeta pm
WHERE pm.meta_key LIKE 'jobsearch_field_experience_%'
LIMIT 20;

-- Find Careerfy meta keys
SELECT DISTINCT meta_key
FROM wp_postmeta
WHERE meta_key LIKE '%careerfy%' OR meta_key LIKE '_careerfy%'
ORDER BY meta_key;

-- Find existing resume/AI-generated storage
SELECT DISTINCT meta_key
FROM wp_postmeta
WHERE meta_key LIKE '%resume%' OR meta_key LIKE '%ai_' OR meta_key LIKE 'jm_%'
ORDER BY meta_key;
```

---

**Status**: Ready for Phase 2 (Admin Settings)  
**Date Created**: 2026-05-13  
**Last Updated**: 2026-05-13
