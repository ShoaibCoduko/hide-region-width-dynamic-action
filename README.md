# Oracle APEX: Hide / Show Regions using Dynamic Actions (Buttons)

<div align="center">

![Preview 1](https://github.com/user-attachments/assets/a5aee49b-905c-4273-a4db-e0684badd101) 
![Preview 2](https://github.com/user-attachments/assets/3fd558cd-818b-485e-a026-560dd2acfb8a)

</div>

## 🎯 Overview

This comprehensive guide demonstrates how to **hide and show APEX regions dynamically** using **Dynamic Actions** triggered by **button clicks**. Learn how to build interactive interfaces with smooth region visibility toggles.

### What You'll Learn
- ✅ Page structure setup (regions + buttons)
- ✅ Dynamic Action configuration and triggers
- ✅ Hide/Show action sequences and order
- ✅ Initialization behavior and runtime results
- ✅ Best practices for region visibility management

---

## 📋 Table of Contents

1. [Page Structure](#page-structure)
2. [Screenshots & Explanations](#screenshots--explanations)
3. [How It Works](#how-the-hideshow-logic-works)
4. [Best Practices](#notes--best-practices)
5. [Author](#author)

---

## 🏗️ Page Structure

This implementation uses a clean, modular architecture with two interactive components:

### Components on the Page

| Component | Type | Purpose |
|-----------|------|---------|
| `click-button` | Button | Trigger for `dynamic-action` |
| `click-button_1` | Button | Trigger for `dynamic-action_1` |
| `hide-region-width-dynamic-action` | Region | Toggle visibility (paired with button 1) |
| `hide-region-width-dynamic-action1` | Region | Toggle visibility (paired with button 2) |

**Key Concept:** Each button controls the visibility of both regions—when one is shown, the other is hidden automatically.

---

## 📸 Screenshots & Explanations

Below is a detailed walkthrough of each Dynamic Action configuration with visual references from the Oracle APEX Page Designer.

> **💡 Tip:** Click on the screenshot numbers to jump to detailed configuration explanations.

### 1️⃣ Show Action Configuration (Region: `hide-region-width-dynamic-action1`)

![Screenshot 1](docs/screenshots/01.png)

**Configuration Details**
| Property | Value |
|----------|-------|
| **Action** | Show |
| **Selection Type** | Region |
| **Region** | `hide-region-width-dynamic-action1` |
| **Sequence** | 20 |
| **Event** | `dynamic-action_1` |
| **Fire When Result Is** | True ✅ |
| **Fire on Initialization** | OFF |

**What Happens**
When button `click-button_1` is clicked:
1. The Dynamic Action `dynamic-action_1` is triggered
2. This **Show** action executes (sequence 20) **after** the Hide action (sequence 10)
3. Region `hide-region-width-dynamic-action1` becomes visible to the user

---

### 2️⃣ Hide Action Configuration (Region: `hide-region-width-dynamic-action`)

![Screenshot 2](docs/screenshots/02.png)

**Configuration Details**
| Property | Value |
|----------|-------|
| **Action** | Hide |
| **Region** | `hide-region-width-dynamic-action` |
| **Sequence** | 10 |
| **Event** | `dynamic-action_1` |
| **Fire When Result Is** | True ✅ |
| **Fire on Initialization** | OFF |

**What Happens**
When button `click-button_1` is clicked:
1. The Dynamic Action `dynamic-action_1` is triggered  
2. **This Hide action executes first** (sequence 10 runs before sequence 20)
3. Region `hide-region-width-dynamic-action` is immediately hidden
4. The Show action (sequence 20) then displays the complementary region

---

### 3️⃣ Dynamic Action Definition: `dynamic-action_1` (Triggered by `click-button_1`)

![Screenshot 3](docs/screenshots/03.png)

**Configuration Details**
| Property | Value |
|----------|-------|
| **Name** | `dynamic-action_1` |
| **Event** | Click |
| **Selection Type** | Button |
| **Button** | `click-button_1` |
| **Event Scope** | Static |
| **Type** | Immediate |
| **Sequence** | 30 |

**What This Does**
- Acts as the **trigger and orchestrator** for region visibility changes
- Listens for clicks on button `click-button_1`
- When clicked, orchestrates a **Hide → Show sequence** (defined via True branch actions with sequences 10 and 20)
- Sequence 30 determines the order relative to other Dynamic Actions on the page

---

### 4️⃣ Show Action Configuration (for `dynamic-action` - with Initialization)

![Screenshot 4](docs/screenshots/04.png)

**Configuration Details**
| Property | Value |
|----------|-------|
| **Action** | Show |
| **Selection Type** | Region |
| **Region** | `hide-region-width-dynamic-...` (truncated) |
| **Event** | `dynamic-action` |
| **Sequence** | 20 |
| **Fire on Initialization** | ON ⭐ |

**Key Difference: Fire on Initialization**
This action has **Fire on Initialization enabled**, meaning:
- The action executes **automatically when the page first loads**
- Not just when the button is clicked
- This sets a **default state** for the page (determines which region is visible at startup)

---

### 5️⃣ Hide Action Configuration (with Fire on Initialization ON)

![Screenshot 5](docs/screenshots/05.png)

**Configuration Details**
| Property | Value |
|----------|-------|
| **Action** | Hide |
| **Event** | `dynamic-action` |
| **Sequence** | 10 |
| **Fire on Initialization** | ON ⭐ |

**Default State Setup**
- When the page **first loads**, this Hide action executes before the Show action (sequence 10 → 20)
- Combined with Screenshot 4, this creates an **initial visibility pattern**
- Sequence ordering ensures Hide always completes before Show

---

### 6️⃣ Dynamic Action Definition: `dynamic-action` (Triggered by `click-button`)

![Screenshot 6](docs/screenshots/06.png)

**Configuration Details**
| Property | Value |
|----------|-------|
| **Name** | `dynamic-action` |
| **Event** | Click |
| **Selection Type** | Button |
| **Button** | `click-button` |
| **Event Scope** | Static |
| **Type** | Immediate |
| **Sequence** | 20 |

**Purpose**
- The **first Dynamic Action** on the page (Sequence 20)
- Listens for clicks on button `click-button`
- Contains Hide/Show actions that toggle region visibility (with Fire on Initialization enabled for default state)

---

### 7️⃣ Runtime: Region `hide-region-width-dynamic-action1` is Visible

![Screenshot 7](docs/screenshots/07.png)

**What You See**
- **Page State:** Browser runtime view with both interactive buttons at the top
- **Visible Region:** `hide-region-width-dynamic-action1` is displayed
- **Content Area:** Large white space ready for content within the region

**Why This State?**
- One of the buttons was clicked, triggering its associated Dynamic Action
- The Hide/Show sequence hid one region while showing this one
- This demonstrates the **final visual result** of the Dynamic Action configuration

---

### 8️⃣ Runtime: Region `hide-region-width-dynamic-action` is Visible

![Screenshot 8](docs/screenshots/08.png)

**What You See**
- **Page State:** Browser runtime view after a different button interaction
- **Visible Region:** `hide-region-width-dynamic-action` is now displayed
- **Toggling in Action:** Demonstrates how the Dynamic Actions successfully switch between regions

**Visual Confirmation**
This screenshot proves that:
- ✅ Both Dynamic Actions are working correctly
- ✅ Clicking different buttons shows/hides the appropriate regions
- ✅ Only one region is visible at any given time (no overlap)

---

## 🔄 How the Hide/Show Logic Works

### Execution Flow

```
User Clicks Button
       ↓
Dynamic Action Triggered
       ↓
Sequence 10: Hide Region A
       ↓
Sequence 20: Show Region B
       ↓
Page Updated - Only Region B Visible
```

### Key Principles

| Principle | Description | Example |
|-----------|-------------|---------|
| **Sequence Order** | Lower sequence numbers execute first | Hide (10) → Show (20) |
| **True/False Branches** | Actions execute based on the condition result | `Fire When Event Result Is = True` |
| **Fire on Init** | Determines if action runs when page loads | Enabled for default states, disabled for click-only |
| **Mutual Exclusivity** | Only one region visible at a time | Hide completes before Show starts |

### Pattern Used Here

For **each button click** (Dynamic Action):
1. Hide one region (Sequence 10)
2. Show the complementary region (Sequence 20)

This ensures:
- **No overlapping regions** displayed simultaneously
- **Predictable transitions** every time
- **Clean user experience** with clear visibility states

---

## 💡 Best Practices & Tips

### ✅ Do This

- **Use Sequence to Control Order**
  - Always use sequence numbers (10, 20, 30...) to enforce execution order
  - Hide first, then Show (Hide = 10, Show = 20)

- **Set Initialization States**
  - Enable "Fire on Initialization" for actions that define the default page appearance
  - This ensures the correct region is visible when the page first loads

- **Use Consistent Naming**
  - Name Dynamic Actions descriptively (e.g., `toggle-regions-button-1`)
  - Use clear region names that indicate their purpose

- **Test Both States**
  - Verify that clicking each button produces the expected visibility
  - Confirm that only one region is visible at a time

### ❌ Avoid These Common Issues

| Problem | Cause | Solution |
|---------|-------|----------|
| **Both regions visible** | Both DAs firing on init OR both shown in True branch | Check Fire on Initialization flags and verify sequences |
| **Neither region visible** | Both regions hidden OR wrong selection in Affected Elements | Verify Region names match exactly |
| **Regions flicker** | Show/Hide happening in wrong order | Ensure Hide sequence < Show sequence (10 < 20) |
| **Button doesn't work** | DA not triggered OR no True/False actions defined | Verify button name and confirm True branch actions exist |

### 🎯 Quick Troubleshooting Checklist

- [ ] Button name matches exactly in Dynamic Action configuration
- [ ] Region names match exactly (case-sensitive in some cases)
- [ ] Sequence numbers are correct (Hide: 10, Show: 20)
- [ ] "Fire When Event Result Is" is set to True
- [ ] At least one Dynamic Action has "Fire on Initialization" enabled
- [ ] Only one Show action for each region in the True branch

---

## 👤 Author

**Shoaib Ahmad**  
*Oracle APEX Developer*

This comprehensive guide and implementation demonstrates professional Dynamic Action configuration techniques within Oracle APEX.

---

## 📚 Related Topics

- [APEX Dynamic Actions Documentation](https://docs.oracle.com/en/database/oracle/application-express/)
- Region Show/Hide Actions
- Dynamic Action Sequences and Execution Order
- Fire on Initialization Behavior
- True/False Branch Conditioning

---

## 📝 Notes

- Make sure to update image file paths if you rename or reorganize the `docs/screenshots/` folder
- This pattern can be extended to 3+ regions by adding more buttons and Dynamic Actions
- Consider using page items and conditions for more complex visibility logic

---

<div align="center">

**Happy coding with Oracle APEX! 🚀**

</div>