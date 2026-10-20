# Exercise 1.2 — Memory Profiling and Script Optimisation

**AI Features:** Memory Profiling · Script Optimisation  
**Duration:** 10 minutes

---

## Business Context

The iFlow you built in Exercise 1.1 contains a Groovy script (`CalculatedDiscount`) with several coding issues introduced deliberately. You will first use **AI Script Optimisation** to detect and fix code-level issues statically — then run the **Simulator with AI Memory Profiling** to surface the runtime memory bottleneck and optimise again with that additional context.

---

## What AI Does Here

| Capability | What happens |
|---|---|
| **Script Optimisation (static)** | Analyses the Groovy script for code quality issues — inefficient patterns, precision risks, empty catch blocks — and proposes targeted fixes |
| **Memory Profiling** | Runs the script steps inside the Simulator with a large payload, measures per-step heap allocation, and flags the most memory-intensive step |
| **Script Optimisation (with profiling context)** | Re-analyses the same script enriched with the profiling results — the AI now has both code and runtime evidence to recommend the streaming fix |

---

## Step 2 — Open the iFlow in Edit Mode

1. Navigate to your iFlow `IN262-0XX-Calculate-Discount`
2. Click **Edit** to enter edit mode

---

## Step 2 — Script Optimisation (Static Analysis)

1. Click the **CalculatedDiscount** step on the canvas → **Processing** tab — click the script filename to open the editor

   ![CalculatedDiscount step selected — Processing tab, script file link highlighted](images/ex01-7.png)

2. The script editor opens — click **Optimize**

   ![Script editor open — Optimize button highlighted](images/ex01-7-1.png)

3. The **AI-Generated Review Report** opens — review the recommendations:

   ![AI Report — Recommendation 1: Inefficient String Concatenation in Loops highlighted](images/ex01-7-3.png)

   **Recommendation 1 — Inefficient String Concatenation in Loops**  
   String concatenation with `+` inside loops creates a new `String` object on every iteration, increasing garbage collection pressure. The fix replaces these with `StringBuilder`.

4. Scroll to **Improved Script** — click the **copy** icon to copy the optimised script to clipboard

   ![AI Report — Improved Script section, copy icon highlighted](images/ex01-7-4.png)

   ![Code copied to clipboard toast](images/ex01-7-5.png)

5. Click inside the **left-hand editor pane** (the original script), select all (`Cmd+A` / `Ctrl+A`), and paste

   ![Script editor left pane highlighted as paste target](images/ex01-7-6.png)

   ![Script editor — improved script pasted, bottom of script visible](images/ex01-7-7.png)

6. Click **Apply**

   ![Apply button highlighted](images/ex01-7-8.png)

> [!NOTE]
> An **Incompatibilities** warning about Groovy version 2.x may appear — this is expected. Click **OK** to dismiss.

   ![Incompatibilities dialog — OK button highlighted, Script changed successfully toast](images/ex01-7-9.png)

7. The AI shows a final summary confirming the script follows SAP Cloud Integration best practices

   ![AI Report — final summary confirming best practices, Close button highlighted](images/ex01-7-10.png)

8. Click **Close** to return to the canvas

   ![Close button highlighted](images/ex01-7-11.png)

---

## Step 3 — Set Up the Simulator

1. Select the arrow before CalculatedDiscount script and Click the **Simulate** icon in the toolbar

   ![Canvas — CalculatedDiscount step selected, Simulate icon in toolbar highlighted](images/ex01-8-1.png)

2. Click the **Start Point** icon on the **CalculatedDiscount** step

   ![Simulation mode — Start Point marker set on CalculatedDiscount step](images/ex01-8-2.png)

3. The **Add Simulation Input** dialog opens — confirm **Enable Memory Insights** is ticked, then click **Upload from File System**

   ![Add Simulation Input dialog — Enable Memory Insights ticked, Upload from File System highlighted](images/ex01-8-3.png)

4. Download [CalculatedDiscount_SimInput.xml](CalculatedDiscount_SimInput.xml), then select it in the file picker and click **Open**

   ![File picker — CalculatedDiscount_SimInput.xml selected, Open highlighted](images/ex01-8-4.png)

5. Confirm **Upload successful** toast → click **OK**

   ![Upload successful toast — OK button highlighted](images/ex01-8-5.png)

6. Click the **End Point** icon on the **Log Summary** step to set the simulation boundary

   ![Canvas — End Point marker set on Log Summary step](images/ex01-8-6.png)

---

## Step 4 — Run Simulation and Profiling

1. Click the **Run Simulation and Profiling** button in the toolbar

   ![Run Simulation and Profiling button highlighted with tooltip](images/ex01-8-8.png)

2. The simulation runs — a progress dialog appears

   ![Running simulation and profiling progress dialog](images/ex01-8-9.png)

3. Confirm the **"Simulation and profiling completed successfully"** toast

   ![Simulation and profiling completed successfully toast](images/ex01-8-10.png)

---

## Step 5 — Review the Memory Profiling Results

1. In the bottom panel, click **Memory Profiling Insights** tab — the `CalculatedDiscount` step is flagged as the top memory consumer

   ![Message Content panel — Memory Profiling Insights tab, CalculatedDiscount flagged, Show More highlighted](images/ex01-8-11.png)

2. Click **Show More** to expand the recommended solution

   ![Memory Profiling Insights — expanded recommended solution, Optimize in Editor button highlighted](images/ex01-8-12.png)

> [!NOTE]
> The AI identifies that parsing the entire XML payload into memory via `XmlSlurper` without streaming causes excessive heap allocation and garbage collection pressure.

3. Click **Optimize in Editor** — the script editor opens with profiling context pre-loaded

   ![Script editor opens — banner confirms optimisation includes memory profiling insights, Optimize button highlighted](images/ex01-8-13.png)

---

## Step 6 — Optimise Again with Memory Profiling Context

The AI now has both static analysis and runtime profiling evidence. It surfaces additional recommendations not visible from code alone:

1. Click **Optimize** — the AI-Generated Review Report opens

   ![AI Report — Recommendation 1: Inefficient String Concatenation highlighted](images/ex01-8-15.png)

   ![AI Report — Recommendation 2: Empty Catch Block for NumberFormatException highlighted](images/ex01-8-16.png)

   **Recommendation 2 — Empty Catch Block**  
   Empty `catch` blocks swallow exceptions silently. The fix adds MPL logging so parsing failures become visible in Message Monitor.

2. Scroll to **Improved Script** — click the **copy** icon

   ![AI Report — Improved Script section, copy icon highlighted](images/ex01-8-17.png)

   ![Code copied to clipboard toast](images/ex01-8-18.png)

3. Click inside the **left-hand editor pane**, select all, and paste

   ![Script editor left pane highlighted as paste target](images/ex01-8-19.png)

4. Click **Apply** → dismiss the Incompatibilities warning → click **Close**

---

## Step 7 — Save as Version and Deploy

1. Click **Save as Version**

   ![Canvas — Save as Version button highlighted](images/ex01-9.png)

2. In the Version Information dialog:
   - **Version:** `1.0.1`
   - **Comment:** `Optimised scripts with memory profiling`
   - Click **Save**

   ![Version Information dialog — version 1.0.1 and comment filled, Save highlighted](images/ex01-9-1.png)

3. Confirm the **"saved as version"** toast — then click **Deploy**

   !["Saved as version" toast — Deploy button highlighted](images/ex01-9-3.png)

4. In the Deploy dialog, confirm **Runtime Profile = Cloud Integration** → click **Deploy**

   ![Deploy dialog — Cloud Integration runtime profile, Deploy button highlighted](images/ex01-9-4.png)

5. The deployment dialog confirms it is triggered → click **OK**

   ![Deployment triggered dialog — OK button highlighted](images/ex01-9-5.png)

6. Confirm the **"successfully deployed"** toast

   !["Successfully deployed" toast](images/ex01-9-6.png)

7. Click **Deployment Status** tab — confirm Deployment Status = **Deployed**, Runtime Status = **Started**

   ![Deployment Status tab — Deployed and Started confirmed, version 1.0.1](images/ex01-9-7.png)

---

## Exercise 1.2 Complete

You applied AI Script Optimisation twice — first statically on the code, then again enriched with runtime memory profiling data — and deployed the fully optimised iFlow.

---

## Key Takeaway

AI assisted at every stage of Exercise 1:
- **Generated** the integration structure from a plain-language description
- **Optimised** the script statically — fixing string concatenation and error handling
- **Profiled** memory usage in simulation — no deployment needed to find the bottleneck
- **Optimised again** with profiling context — the AI identified the streaming fix only visible at runtime

---

*Next: [Exercise 2 — AI Error Resolution](../../ex2/README.md)*
