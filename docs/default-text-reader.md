# שילוב כקורא ברירת מחדל באוצריא

מבוסס על `docs/plugin-sdk/DEFAULT_TEXT_READER.md` בריפו של אוצריא (ענף dev).

- הפעלה: `reader.setDefaultTextReader({enabled})` מתוך מחוות משתמש (מתג בהגדרות; ניסיון אוטומטי בלחיצה הראשונה).
- מופע משובץ מזוהה ב-`plugin.boot` לפי `reader.embedded`; הספר נטען לפי `bookId`/`bookUid`, קפיצה ל-`currentIndex`, הדגשת `searchQuery`.
- `reader.textReaderNavigate`: ניווט; נשמר ב-`PEND` אם הספר עוד בטעינה. אירועים של ספר אחר מסוננים (`sameBook`).
- `reader.textReaderCommand`: `advance`, `settings`, `display` (גודל אות/ניקוד).
- `reader.reportTextReaderLocation({index, ref})`: אחרי גלילה (debounce 600ms), `ref` = כותרת תוכן העניינים הקרובה.
- `reader.setTextReaderFontSize`: בשינוי גודל אות במצב משובץ (טווח 14–60).
- חזרה לעיון רגיל: `reader.openBook({..., reader:'native'})`.
- חל רק על ספרי text; PDF/EPUB/DOCX נשארים בקורא המובנה.

## למה 1.4.2 לא הפעיל את הקורא
- אוצריא בודקת `navigator.userActivation.isActive`. ההפעלה רצה על `pointerdown`, שבמגע אינו נחשב מחוות משתמש, ולפני כן חיכתה ל-`getDefaultTextReader`. עכשיו: `click`/`keydown`, וקריאה ישירה ל-`setDefaultTextReader`.
- המתג בהגדרות הציג "מופעל" לפי ברירת המחדל המקומית גם כשבאוצריא היה כבוי, כך שאי אפשר היה להפעיל. עכשיו הוא משקף את `getDefaultTextReader`.
- כפתור שדה האיתור דורש `search.field_actions` + `contributes.startup.searchFieldActions` ו-`minAppVersion` 0.9.99; ב-1.4.2 שניהם חסרו.
