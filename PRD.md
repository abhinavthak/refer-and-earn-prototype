# PRD: Refer and Earn

**Status:** Prototype ready for review
**Live prototype:** https://abhinavthak.github.io/refer-and-earn-prototype/
**Scope of this document:** Referrer site, Referrer dashboard, Friend's referral link. The admin and white-label screen is covered only where the other three surfaces depend on its rules.

---

## 1. Summary

Refer and Earn lets anyone share a personal invite link for an online university. A friend who opens the link gets a fee discount (default 10%) applied to every programme. When that friend enrols and the refund window passes, the referrer gets an e-gift card automatically. Gift card value grows with the number of friends who enrol.

The programme has three user-facing surfaces:

| Surface | Who uses it | Job |
|---|---|---|
| Referrer site | Prospective referrers | Explain the offer, show what they can earn, and create their invite link |
| Referrer dashboard | Signed-up referrers | Share the link, track every friend, see gift cards earned and scheduled |
| Friend's referral link | Invited friends | Show the discounted price and book a counselling session |

## 2. Problem

- Referrals today rely on codes and manual entry, which leak attribution and create disputes.
- Referrers can't see where their friends are in the admissions journey or when they will be rewarded.
- Invited friends don't see their discounted price until late in the process, so the invite feels abstract.
- Paying rewards before the refund window closes creates clawback problems.

## 3. Goals and success metrics

| Goal | Metric | Target (to be set with the business) |
|---|---|---|
| More people start referring | Visitor to link created rate on the referrer site | TBD |
| Referrers share actively | Share actions per referrer in the first 7 days | TBD |
| Invites convert | Friend link open to counselling form submit rate | TBD |
| Referrals become admissions | Referred lead to enrolment rate vs. other channels | TBD |
| Low support load | Referral related tickets per 100 enrolments | TBD |
| Clean payouts | Gift cards clawed back after sending | 0 |

**Non-goals for v1:** cash payouts, referral codes, manually adding a friend after the fact, multi-level referrals.

## 4. Programme rules (shared by all surfaces)

These are configurable per university in the admin screen. Defaults shown.

| Rule | Default |
|---|---|
| Friend discount | 10% off the total programme fee |
| Lock period (refund window) | 30 days after the friend pays the fee |
| Reward type | E-gift card only. No cash. |
| Reward tiers, by enrolment order | Starter: friend 1 gets ₹1,000. Silver: friends 2 to 6 get ₹5,000 each. Gold: friends 7 to 11 get ₹7,000 each. |
| After the last tier | Stop rewarding (cap of 11 gift cards, ₹61,000 total). Alternative setting: keep paying the last tier's rate. |

**Attribution and eligibility**

1. A referral counts only when the friend signs up through the referrer's link. No codes, no manual adds.
2. A referrer cannot refer their own mobile number.
3. One referrer per friend. Duplicates and people who are already active leads or students are rejected.
4. Tier position is decided by **enrolment date**, not referral date.
5. A gift card is **reserved** when the friend pays the fee and **unlocks** after the lock period.
6. A refund inside the lock period cancels that gift card, and later gift cards move up one tier position. Gift cards already sent are never taken back.
7. Payout is automatic. The day a gift card unlocks it is sent to the referrer's email and by SMS. There is no claim step.
8. Ops can change the amount of a scheduled gift card before it is sent. Sent amounts are frozen.

**Friend journey stages**

`Lead registered → Counselling → Applied → Enrolled → Reward eligible → Paid (gift card sent)`
Void states: `Not eligible` (duplicate or existing lead) and `Refunded`.

---

## 5. Referrer site

The public landing page that sells the programme and creates the invite link.

### 5.1 User stories

- As a visitor, I want to understand in seconds what I get and what my friend gets.
- As a visitor, I want to see how much I could earn for a given number of friends.
- As a visitor, I want to create my link without long forms or enrolling myself.
- As a visitor, I want to trust that this is real (stories, top referrers, clear rules).

### 5.2 Page sections, in order

| # | Section | Content and behaviour |
|---|---|---|
| 1 | Header | University logo, main site nav (Home, About, programmes, Academics, Admissions, Refer and Earn, Blogs), admissions banner with Apply Now, and a **Start referring** button. |
| 2 | Hero | Headline "Turn referrals into rewards". Value line "Up to ₹7,000 for every friend who enrols". Explains the friend's 10% saving. Primary CTA **Start referring**, secondary **See how it works**. Visual shows total possible earnings (₹61,000 for 11 friends), a sample friend saving (₹18,000 on MBA) and the 30-day unlock. A rotating note cycles through live-style events (for example "Gift card sent: Sneha enrolled in BCA, ₹5,000"). |
| 3 | Activity ticker | Scrolling strip of recent activity with photos, such as "Kavya from Pune: ₹5,000 gift card sent". |
| 4 | How it works | Three cards without numbers: Get your link and share it. Your friend enrols and saves. Get your gift card (reserved on fee payment, sent automatically after the 30-day refund window). |
| 5 | Share your link | Inline sign-up card: name, mobile, Send OTP, OTP, accept terms, **Create my link**. Benefits beside it: 10% off built in, share anywhere, track every friend. Once signed up, the same card turns into the share card (see 6.4). |
| 6 | Power referrers | Quarterly leaderboard: top 3 on a podium with photos, ranks 4+ in a list, each showing friends enrolled and gift card total. Ends with "Think you can make it to the top?" and a CTA. |
| 7 | Rewards simulator | Slider for number of friends. Shows total gift cards (for example ₹26,000 for 6 friends) and a per-friend grid coloured by tier (Starter, Silver, Gold). Copy stays neutral about how gift cards are spent: "Delivered as e-gift cards. Spend them however you like." |
| 8 | How your friend joins | Three steps with an image: they open your link, they pick a programme and see their price, they book a counselling session. Link to **Preview your friend's page**. |
| 9 | Stories | Testimonial cards with photo, name, role, star rating and quote. |
| 10 | Programmes | Table of programmes with duration, full fee and fee with 10% off. |
| 11 | Closing CTA | "Ready to start earning?" with **Start referring** and **See your friend's page**. |
| 12 | FAQ | Accordion, first item open. Covers who can refer, rewards per tier, when a gift card unlocks, how it is delivered, what the friend gets, how attribution works, earning cap, existing leads, refunds. Answers are generated from the live rule config so they never drift from the rules. |

### 5.3 Sign-up (create my link)

- Fields: Your name, Mobile (+91, 10 digits starting 6 to 9), OTP, terms checkbox.
- Available inline (section 5) and as a modal from any **Start referring** button.
- Validation messages: "Enter your name.", "Enter a valid 10-digit mobile number.", "Tap Send OTP to verify your number.", "Incorrect OTP. Try again.", "Please accept the programme terms."
- On success: the link is created in the format `refer.<domain>/r/<FIRSTNAME><discount>` (for example `/r/ABHINAV10`) and the user lands on the dashboard with a "Your link is ready!" toast.
- No enrolment or existing account is needed. Anyone 18+ with a valid Indian mobile can refer.

### 5.4 Requirements

- Every rupee amount, percentage, tier name and lock period on the page comes from config. Nothing is hard-coded.
- If the cap rule is on, show a cap notice ("Reward cap: 11 gift cards, ₹61,000 maximum").
- A sticky CTA appears on scroll when the main CTA is out of view.
- Mobile first. All sections work at 390px width with no horizontal scroll.
- Copy is plain language, short sentences, no dashes.

---

## 6. Referrer dashboard

The logged-in home for referrers. Reachable directly from the site header and after sign-up.

### 6.1 User stories

- As a referrer, I want to share my link in one tap on the app my friends use.
- As a referrer, I want to see each friend's stage without calling the university.
- As a referrer, I want to know exactly when each gift card will be sent and for how much.
- As a referrer, I want to know how far I am from the next tier and from the cap.

### 6.2 Layout

1. **Greeting:** "Hi <first name>. Track your friends and gift cards in one place." with **Share my link**.
2. **Summary tiles:** Gift cards earned (sent + scheduled), Friends who joined (excludes void), Friends enrolled (shown as "X of 11" when a cap applies).
3. **Your friends table** with filter tabs: All, In progress, Enrolled, Gift card sent.
4. **Gift card levels** card showing tiers, the referrer's current position and progress to the cap. Same height as the friends card beside it.
5. **Share card** (see 6.4).

### 6.3 Friends table

| Column | Content |
|---|---|
| Friend | Photo or initial avatar, name, programme |
| Programme | Programme name |
| Stage | Current journey stage |
| Progress | Visual progress through Joined, Counselling, Applied, Enrolled |
| Gift card | Amount once enrolled. "Not earned" before enrolment. "No gift card" for void. |
| Status | Pill: **Pending** (before enrolment), **Scheduled** (in lock period), **Sent**, **Void** |
| Date | Send date for scheduled cards (for example "25 Oct"), shown in its own column and not mixed into the status |
| Action | **Remind** for in-progress friends: opens WhatsApp with a pre-filled nudge |

Sort order: scheduled cards first, then in progress, then sent, then void. Void rows show the reason (already an active lead, refunded within window).

### 6.4 Sharing

- Read-only link field with **Copy**.
- Channels: WhatsApp, SMS, Email, More (native share sheet, falling back to copying the message), plus Telegram, X and Facebook on the dashboard.
- **Preview** opens the friend's page as the friend will see it.
- Editable share message (collapsed under "Edit share message"). Default: "Hey! I'm referring you to <University>. Join through my link and get 10% off your online degree: <link>"
- Every channel uses the edited message.

### 6.5 Requirements

- Tier amounts are recalculated by enrolment order whenever a refund happens, so the table always reflects the current position.
- When a lock period ends, the row moves to **Sent** on its own and the referrer receives the gift card by email and SMS. No claim button in the default flow.
- Delivery email can be set from the dashboard.
- Empty state for a new referrer points to sharing the link.
- Table becomes a stacked card list on mobile. Tags and amounts never wrap onto a second line.

---

## 7. Friend's referral link

The page a friend lands on from `refer.<domain>/r/<CODE>`.

### 7.1 User stories

- As an invited friend, I want to know who invited me and that the discount is real.
- As an invited friend, I want to see my price for the programme I care about before giving my details.
- As an invited friend, I want to talk to someone before committing.

### 7.2 Layout

1. **Invite banner:** referrer's initial or photo, "<Name> invited you", "10% off applied". No gap above it.
2. **Price card:** headline "Get 10% off your online degree. Pick a programme to see your price." Programme chips (MBA, MCA, BBA, BCA, M.Com, B.Com, MA (JMC), BA). Selected programme shows mode and duration, "You save ₹X", "You pay ₹Y" with the original fee struck through, and "total fee, 10% invite discount applied". Highlights: UGC-entitled online degree, live and recorded classes, placement support.
3. **Counselling form** ("Your details. We'll call you on this number."):
   - Mobile No. (+91)
   - Name
   - Email Id
   - Highest qualification: 12th pass, Diploma, Graduate, Postgraduate, Other
   - Course (pre-filled from the selected programme)
   - CTA: **Claim 10% off**
4. **FAQ: Questions about your invite.** How the discount works, no code needed, session is free, what happens after submitting, changing programme later, recognition of degrees, already spoke to the university, does my friend get anything.

### 7.3 Behaviour and validation

- Discount is applied from the link alone. No code entry anywhere.
- Validation: "Enter a valid 10-digit mobile number.", "Enter your name.", "Enter a valid email address.", "Select your highest qualification."
- Self-referral blocked: "Referrers can't use their own link."
- Duplicate blocked: "This number is already registered with a referral."
- Existing active leads are accepted as a form submit but marked Not eligible for the referrer (no gift card), and the friend's counsellor explains what applies.
- Success state: "You're in, <first name>! Your 10% discount on <programme> is locked to <masked mobile>. You pay ₹Y instead of ₹X. A counsellor will call within 24 hours."
- On submit, the friend appears on the referrer's dashboard as **Lead registered** and in the CRM with source = referral link and referrer ID.
- Sticky **Claim 10% off** CTA on mobile when the form is out of view.

---

## 8. Data and integrations

| Need | Detail |
|---|---|
| Referrer record | ID, name, mobile (verified by OTP), email, code, link clicks |
| Referral record | Friend name, mobile, email, qualification, programme, referrer ID, source, stage, enrolment date, tier position, gift card amount, payout date, status, void reason |
| CRM | Create lead with referral source on friend form submit. Receive stage updates (counselling, applied, enrolled, refunded). |
| Fee system | Payment date sets enrolment date and starts the lock period. Refund events void the gift card. |
| OTP / SMS | OTP for referrer sign-up. SMS for gift card delivery and friend confirmations. |
| Gift card provider | Issue e-gift card on unlock day, deliver by email and SMS. |
| Link tracking | Count clicks per referrer link. |
| Config | Per-university white-label: name, logo, colours, referral domain, support phone, discount %, lock days, tiers, cap behaviour. |

## 9. Edge cases

| Case | Expected result |
|---|---|
| Friend opens link, signs up later from another channel | Not attributed. Only link sign-ups count. |
| Two referrers invite the same friend | First valid sign-up through a link wins. |
| Friend refunds on day 20 | Gift card cancelled, later friends move up a tier position, dashboard updates. |
| Friend refunds on day 31 | Gift card already sent and kept. |
| Referrer reaches the cap | Further friends still get 10% off. No further gift cards. Dashboard shows cap reached. |
| Ops edits a scheduled amount | Dashboard shows the new amount. Not editable after sending. |
| Referrer opens their own link | Form blocks their own mobile number. |

## 10. Open questions

1. Final tier amounts, cap and lock period per university.
2. Gift card provider and which brands are offered.
3. Do we need KYC or PAN for referrers above a yearly reward value?
4. Leaderboard: real data with consent, or hidden until there is enough volume?
5. Should referrers be able to choose a custom link code?
6. Fraud checks beyond self-referral and duplicates (device, IP, velocity limits).

## 11. Release plan

| Phase | Scope |
|---|---|
| v1 | Referrer site, sign-up with OTP, dashboard with sharing and tracking, friend link with counselling form, automatic gift card payout, admin config |
| v1.1 | Referrer email preferences, nudge reminders from the dashboard, link click analytics |
| v2 | Leaderboard with real data, campaigns (time-limited bonus tiers), more payout options |

## 12. Prototype notes

- Single `index.html`, no build step. Brand "Northbridge University Online" is fictional.
- Demo OTP is `1234`. Forms don't send data anywhere. All people and numbers are sample data.
- Use the **View** bar at the top to switch between the three surfaces and the admin screen. The admin screen can fast-forward days to watch gift cards unlock and send.
