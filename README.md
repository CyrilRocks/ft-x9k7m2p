# 💰 FinTrack — Family Expense Manager

A self-contained, offline-first personal finance tracker built specifically for
managing multi-person family expenses. All data stays in your browser — no
server, no login, no subscriptions.

---

## 🚀 Quick Start

**Just open the file in your browser:**
```
Double-click index.html
```
That's it! The app works immediately with no setup required.

> **Tip:** Bookmark this file or create a desktop shortcut for quick access.

---

## 📱 Install as Phone App (PWA)

1. Open `index.html` hosted on a server (GitHub Pages, local server, etc.)
2. In Chrome: tap **⋮ → Add to Home screen**
3. The app icon appears on your phone — use it like a native app

### Free Hosting on GitHub Pages
1. Create a GitHub repo, push the `expense-portal/` folder
2. Go to Settings → Pages → Deploy from main branch
3. Access at `https://yourusername.github.io/repo-name/`

---

## 🎯 Features

| Feature | Description |
|---------|-------------|
| **Quick Add** | Floating + button, 2-3 taps to log an expense |
| **Dashboard** | Monthly overview, category/person charts |
| **Expense List** | Search, filter by category/person/account |
| **Analytics** | 6-month trends, daily spending, account breakdown |
| **Recurring** | Auto-generate monthly EMIs, SIPs, bills |
| **Budgets** | Set limits per category, visual warnings |
| **Export/Import** | JSON backup + CSV for spreadsheet analysis |
| **Dark/Light** | Toggle theme anytime |
| **Offline** | Works without internet (data is local) |
| **Keyboard** | Press `N` to quickly add expense, `Esc` to close modals |

---

## 🔧 Customization Guide

All customizations are in the **CONFIG** object at the top of the JavaScript
section in `index.html`. Search for `const CONFIG =` to find it.

### Add a New Family Member

```javascript
// In CONFIG.people, add the name:
people: ['Me', 'Mother', 'Father', 'Brother', 'Sister'],

// In CONFIG.income, add their monthly income:
income: {
  'Me': 67500,
  'Mother': 70000,
  'Father': 0,
  'Brother': 0,
  'Sister': 30000,   // ← new
},
```

### Add a New Category

```javascript
// In CONFIG.categories, add an entry:
categories: [
  // ... existing categories ...
  { name: 'Pet Care', emoji: '🐕', color: '#84cc16' },  // ← new
],
```

### Add a Payment Account

```javascript
// In CONFIG.accounts, add it:
accounts: [
  'My Bank Account',
  "Mother's Account",
  'Credit Card 1',
  'Credit Card 2',
  'Cash',
  'UPI',
  'Credit Card 3',  // ← new
],
```

### Change Default Budgets

```javascript
// In CONFIG.defaultBudgets:
defaultBudgets: {
  'Food & Dining': 5000,
  'Entertainment': 2000,
  'Shopping': 3000,
  'Pet Care': 1500,   // ← budget for new category
},
```

### Change Currency

```javascript
currency: '$',  // Change from ₹ to $ or any symbol
```

### Change Colors/Theme

Edit the CSS variables at the top of the `<style>` section:
```css
:root {
  --accent: #00d4aa;        /* Main accent color */
  --bg-primary: #0a0a1a;    /* Background color */
  --bg-card: #1a1a2e;       /* Card background */
  /* ... more variables ... */
}
```

---

## 📊 Understanding the Analytics

### Dashboard
- **Total Income**: Combined monthly income of all earning members
- **Total Expenses**: Sum of all expenses logged this month
- **Net Savings**: Income minus expenses (aim for positive!)
- **Investments**: SIP + RD/FD contributions this month

### Category Chart (Doughnut)
Shows where your money is going. Large slices = areas to review.

### Person-wise Chart (Bar)
Who is spending how much. Helps balance family expenses.

### Monthly Trend (Line)
6-month history. Spot if spending is increasing over time.

### Daily Spending (Bar)
See which days you spend most. Identify patterns.

---

## 💾 Data Safety

### Where is data stored?
In your browser's **localStorage**. It persists across sessions but is tied to:
- The specific browser you use
- The specific device

### Regular Backups
**Export your data weekly!** Go to Settings → Export Data.

The JSON backup file contains everything — expenses, recurring items, budgets,
and settings. You can import it back anytime.

### CSV Export
Use Settings → Export CSV to open your data in Excel/Google Sheets for deeper
analysis.

---

## 🗂️ Code Structure

The entire app is in a single `index.html` file, organized in clearly marked
sections:

```
index.html
├── <style>           CSS (themes, layout, components)
│   ├── Theme Variables
│   ├── Base Styles
│   ├── Layout (sidebar, main)
│   ├── Cards & Grid
│   ├── Buttons
│   ├── Modal
│   ├── Forms
│   ├── Table / List
│   ├── Recurring
│   ├── Toast Notifications
│   ├── Budget Bars
│   ├── Settings
│   └── Responsive
│
├── <body>            HTML structure
│   ├── Sidebar Navigation
│   ├── Dashboard Page
│   ├── Expenses List Page
│   ├── Analytics Page
│   ├── Recurring Page
│   ├── Budgets Page
│   ├── Settings Page
│   ├── FAB (Floating Action Button)
│   ├── Add/Edit Expense Modal
│   └── Recurring Expense Modal
│
└── <script>          JavaScript
    ├── CONFIG         ← Edit this for customization
    ├── DB (Storage)   ← localStorage CRUD operations
    ├── Utils          ← Formatting helpers
    ├── Navigation     ← Page routing
    ├── Dashboard      ← Dashboard rendering + charts
    ├── Expense List   ← List with filters
    ├── Analytics      ← All chart renderers
    ├── Recurring      ← Recurring expense management
    ├── Budgets        ← Budget tracking
    ├── Settings       ← Settings page + income/budget saving
    ├── Modals         ← Add/edit expense & recurring
    ├── Export/Import  ← Data backup & restore
    ├── Theme          ← Dark/light toggle
    ├── Toast          ← Notification system
    └── Init           ← App initialization
```

---

## 🛠️ Common Modifications

### Add a new page/section
1. Add a `<button class="nav-link">` in the sidebar
2. Add a `<div class="page" id="page-newpage">` in the main area
3. Add a `case 'newpage':` in `renderCurrentPage()`
4. Write a `renderNewPage()` function

### Add a new chart
1. Add a `<canvas id="myChart">` in the HTML
2. Write a render function using Chart.js:
```javascript
function renderMyChart() {
  destroyChart('myChart');
  chartInstances.myChart = new Chart(document.getElementById('myChart'), {
    type: 'bar', // or 'line', 'pie', 'doughnut'
    data: { labels: [...], datasets: [{ data: [...] }] },
    options: { responsive: true, maintainAspectRatio: false }
  });
}
```

### Add a new field to expenses
1. Add the input in the expense modal HTML
2. Include the field in `saveExpense()` → `data` object
3. Display it in `renderExpenseItem()`
4. Add it to `exportCSV()` headers and rows

---

## 📋 Recommended Monthly Workflow

1. **Start of month**: Go to Recurring → click "Auto-Generate This Month"
2. **Daily**: Tap + to log expenses as they happen (takes 5-10 seconds each)
3. **Weekly**: Check Dashboard for spending overview
4. **End of month**: Review Analytics, check Budget tracker
5. **Monthly**: Export backup (Settings → Export Data)

---

## 🔮 Future Enhancement Ideas

When you need new features, here are some ideas you can implement:

- [ ] **Split expenses**: Mark an expense as split between people
- [ ] **Tags**: Add custom tags for more granular filtering
- [ ] **Goals**: Set savings goals with progress tracking
- [ ] **Credit card billing cycle**: Track billing periods separately
- [ ] **Receipts**: Attach photos of receipts (as base64 in localStorage)
- [ ] **Reports**: Generate printable monthly financial reports
- [ ] **Multi-currency**: Support transactions in different currencies
- [ ] **Notifications**: Browser notifications for budget warnings

---

## ⚡ Tips for Consistent Usage

1. **Log immediately**: Add expenses right when you spend (don't wait)
2. **Use recurring**: Set up all fixed expenses once, auto-generate monthly
3. **Review weekly**: Quick dashboard check every Sunday (2 minutes)
4. **Export monthly**: Backup your data on the 1st of each month
5. **Keyboard shortcut**: Press `N` to quickly add — saves time on desktop

---

## 📄 License

This is your personal tool. Use, modify, and distribute freely.
