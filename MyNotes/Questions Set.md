---

## tags: [dashboard]

# 🧠 CP Revision Dashboard

> এই note-টা vault-এর যেকোনো জায়গায় রাখো (root-এ রাখলে সহজে খুঁজে পাবে)। নিচের সব query কাজ করার জন্য **Dataview plugin** (community plugins থেকে install করে নাও, না থাকলে) দরকার। ধরে নেওয়া হয়েছে তোমার সব CP problem-note একটা folder-এ আছে — নিচে `"CP-Problems"` জায়গায় তোমার আসল folder-এর নাম বসাও।

---

## 🔴 এখনো দুর্বল — এগুলো আগে revise করো

```dataview
TABLE date, difficulty, tags
FROM "CP-Problems"
WHERE confidence = "🔴"
SORT date ASC
```

## 🟡 মাঝামাঝি — একটু ঝালাই দরকার

```dataview
TABLE date, difficulty, tags
FROM "CP-Problems"
WHERE confidence = "🟡"
SORT date ASC
```

## 📅 গত ৭ দিনে solve করা (Day-3 revision-এর জন্য)

```dataview
TABLE date, confidence, tags
FROM "CP-Problems"
WHERE date >= date(today) - dur(7 days)
SORT date DESC
```

## 📅 ঠিক ৭ দিন আগে solve করেছিলে (আজ Full-resolve করার কথা)

```dataview
TABLE date, confidence, tags
FROM "CP-Problems"
WHERE date = date(today) - dur(7 days)
```

## 🏷️ Trick অনুযায়ী গ্রুপ করা (pattern library হিসেবে ব্যবহার করো)

```dataview
TABLE tags, date, confidence
FROM "CP-Problems"
GROUP BY tags
```

## ✅ Solved কিন্তু কখনো 🟢 হয়নি (এগুলো long-term এ ভুলে যাবে, priority দাও)

```dataview
LIST
FROM "CP-Problems"
WHERE status = "solved-with-help" AND confidence != "🟢"
SORT date ASC
```

---

## 📋 Revision Workflow (প্রতিদিন যা করবে)

1. **প্রথমে এই dashboard খোলো।**
2. **🔴 আর 🟡 section-এর প্রতিটা note-এর জন্য:**
    - Note **বন্ধ রাখো**।
    - শুধু title দেখে ৩০ সেকেন্ড ভাবো — "এই problem-এ কী trick লেগেছিলো?"
    - তারপর note খুলে **"এক লাইনে কী miss করেছিলাম"** অংশ পড়ে মিলাও।
    - মনে পড়ে গেলে → confidence 🟡 → 🟢 করে দাও।
    - মনে না পড়লে → note-এর "যেই trick টা কাজে লাগলো" অংশ পড়ে আবার ৩ দিন পর revisit-এ রাখো।
3. **সপ্তাহে একবার:** "Trick অনুযায়ী গ্রুপ করা" section থেকে একটা tag বেছে, সেই tag-এর সব problem একসাথে scroll করে pattern মিলাও।
4. **Day-7 section-এ যেগুলো আসবে:** সেগুলোর জন্য **Full resolve** করো — problem statement আবার পড়ে, কাগজে/মাথায় পুরো solution নতুন করে বানানোর চেষ্টা করো, note না দেখে।

---

## 📝 নতুন problem note বানানোর সময় এই template কপি করো

```markdown
---
tags: [cp/TRICK-NAME-HERE]
status: solved-with-help
difficulty: Div2-C
date: {{date:YYYY-MM-DD}}
source: 
confidence: 🔴
---

# Problem name

## এক লাইনে কী miss করেছিলাম


## Trigger phrase (পরের বার এই lines দেখলে এই note মনে করবো)
- 

## যেই trick টা কাজে লাগলো
1. 

## আমার ভুল ধারণা কী ছিল


## Related problems
[[]]

## Confidence
🔴 / 🟡 / 🟢
```

> **Tip:** Obsidian-এর **Templater plugin** ব্যবহার করলে এই template-টা `Templates/` folder-এ save করে, নতুন note বানানোর সময় `Ctrl/Cmd + P → Templater: Insert Template` দিয়ে সরাসরি বসিয়ে নিতে পারবে, আর `{{date:YYYY-MM-DD}}` স্বয়ংক্রিয়ভাবে আজকের তারিখ বসিয়ে দেবে।

---

## ⚠️ Setup checklist

- [ ] Dataview plugin install করা আছে? (Settings → Community plugins → Browse → "Dataview")
- [ ] উপরের সব query-তে `"CP-Problems"` জায়গায় তোমার আসল folder-এর নাম বসিয়েছো?
- [ ] প্রতিটা problem note-এ `confidence`, `date`, `status` frontmatter field ঠিকভাবে বসাচ্ছো? (query গুলো এই field-এর নামের উপর নির্ভর করে)
- [ ] (Optional) Spaced Repetition plugin install করে flashcard format ব্যবহার করতে চাও কিনা ঠিক করেছো?