"""
OT Pre-Surgery Equipment Checklist CLI App
Author: Kusum | Perfusion Technology + Data Analytics Portfolio
Version: 1.0
"""

import csv
import os
from datetime import datetime

# ─────────────────────────────────────────────────────────────────────────────
# CHECKLIST DATA
# ─────────────────────────────────────────────────────────────────────────────

CHECKLIST = {
    "Airway & Ventilation": [
        "Endotracheal tubes (assorted sizes)",
        "Laryngoscope with working blade",
        "Bag-Valve-Mask (BVM) assembled",
        "Suction machine functional",
        "Oxygen supply connected & pressure checked",
        "Ventilator settings configured",
        "PEEP valve in place",
        "End-tidal CO2 monitor connected",
    ],
    "Anesthesia Equipment": [
        "Anesthesia machine checked (O2, N2O, Air)",
        "Vaporizer filled (Sevoflurane/Isoflurane)",
        "Breathing circuit assembled & leak-tested",
        "Soda lime canister checked (not exhausted)",
        "Gas scavenging system connected",
        "IV access confirmed (gauge & patency)",
        "Emergency drugs drawn up and labeled",
    ],
    "Cardiac & Perfusion (CPB)": [
        "Heart-Lung Machine primed",
        "Oxygenator connected & inspected",
        "Arterial & venous lines de-aired",
        "Roller pump calibrated",
        "Cardioplegia solution prepared",
        "ACT machine ready",
        "Heparin dose calculated & drawn",
        "Cell saver (autotransfusion) set up",
        "Temperature probes placed",
        "Emergency hand crank accessible",
    ],
    "Monitoring Equipment": [
        "ECG leads attached & signal clear",
        "Pulse oximeter functional",
        "NIBP cuff connected",
        "Arterial line (invasive BP) zeroed",
        "Central venous pressure (CVP) line checked",
        "Temperature monitor active",
        "Urinary catheter & urine bag connected",
        "Defibrillator charged & pads placed",
    ],
    "Surgical Instruments & Sterility": [
        "Surgical instrument tray counted (swab & needle count)",
        "Sterile drapes applied correctly",
        "Diathermy (electrosurgical unit) tested",
        "Irrigation fluid (saline) available",
        "Blood products ordered & available",
        "Implants/prosthetics confirmed (if applicable)",
    ],
    "Safety & Compliance": [
        "Patient identity verified (name, DOB, MRN)",
        "Consent form signed & present",
        "Surgical site marked (if required)",
        "Allergies communicated to team",
        "Time-out completed with full team",
        "Fire extinguisher location noted",
        "Emergency exit route clear",
    ],
}

LOG_FILE = "ot_checklist_log.csv"

# ─────────────────────────────────────────────────────────────────────────────
# HELPERS
# ─────────────────────────────────────────────────────────────────────────────

def clear():
    os.system("cls" if os.name == "nt" else "clear")

def separator(char="─", width=65):
    print(char * width)

def header():
    clear()
    separator("═")
    print("   🏥  OT PRE-SURGERY EQUIPMENT CHECKLIST VALIDATOR")
    print("       Perfusion Technology | Clinical Safety Tool")
    separator("═")
    print()

def color(text, code):
    """ANSI color codes for terminal output."""
    return f"\033[{code}m{text}\033[0m"

def green(t):  return color(t, "92")
def red(t):    return color(t, "91")
def yellow(t): return color(t, "93")
def cyan(t):   return color(t, "96")
def bold(t):   return color(t, "1")

# ─────────────────────────────────────────────────────────────────────────────
# CORE LOGIC
# ─────────────────────────────────────────────────────────────────────────────

def run_checklist():
    """Walk through all categories and items interactively."""
    results = {}   # { "Category > Item": "PASS" / "FAIL" / "NA" }
    total = sum(len(items) for items in CHECKLIST.values())
    checked = 0
    failed_items = []

    print(bold(f"  Total items to validate: {total}"))
    print(f"  Press  {green('Y')} = Present/OK   {red('N')} = Missing/Fail   {yellow('S')} = Skip\n")
    input("  Press ENTER to begin checklist...")

    for category, items in CHECKLIST.items():
        header()
        print(bold(cyan(f"  ▶  SECTION: {category}")))
        separator()

        for item in items:
            while True:
                answer = input(f"  {'✔' if False else '□'}  {item}\n     [Y/N/S] → ").strip().upper()
                if answer in ("Y", "N", "S"):
                    break
                print(red("     Invalid input. Enter Y, N, or S."))

            key = f"{category} > {item}"
            if answer == "Y":
                status = "PASS"
                checked += 1
                print(green("     ✔ OK\n"))
            elif answer == "N":
                status = "FAIL"
                failed_items.append(item)
                print(red("     ✘ FLAGGED\n"))
            else:
                status = "SKIPPED"
                print(yellow("     — Skipped\n"))

            results[key] = status

    return results, checked, total, failed_items


def show_summary(results, checked, total, failed_items, operator, patient_id, surgery_type):
    """Display session summary."""
    header()
    passed = sum(1 for v in results.values() if v == "PASS")
    failed = sum(1 for v in results.values() if v == "FAIL")
    skipped = sum(1 for v in results.values() if v == "SKIPPED")
    score = round((passed / total) * 100, 1)

    separator("═")
    print(bold("  📋  CHECKLIST SUMMARY"))
    separator("═")
    print(f"  Operator      : {operator}")
    print(f"  Patient ID    : {patient_id}")
    print(f"  Surgery Type  : {surgery_type}")
    print(f"  Timestamp     : {datetime.now().strftime('%Y-%m-%d %H:%M:%S')}")
    separator()
    print(f"  {green('✔ Passed')}  : {passed}")
    print(f"  {red('✘ Failed')}  : {failed}")
    print(f"  {yellow('— Skipped')} : {skipped}")
    print(f"  Compliance    : {green(f'{score}%') if score >= 90 else red(f'{score}%')}")
    separator()

    if failed_items:
        print(red(bold("  ⚠  ITEMS REQUIRING ATTENTION:")))
        for item in failed_items:
            print(f"     • {item}")
        separator()

    if score == 100:
        print(bold(green("  ✅  OT IS CLEARED FOR SURGERY")))
    elif score >= 90:
        print(bold(yellow("  ⚠   PROCEED WITH CAUTION — Address flagged items")))
    else:
        print(bold(red("  🚫  DO NOT PROCEED — Critical items missing")))
    separator("═")


def save_to_csv(results, operator, patient_id, surgery_type):
    """Append session log to CSV file."""
    timestamp = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    file_exists = os.path.isfile(LOG_FILE)

    with open(LOG_FILE, mode="a", newline="", encoding="utf-8") as f:
        writer = csv.writer(f)
        if not file_exists:
            writer.writerow(["Timestamp", "Operator", "Patient_ID", "Surgery_Type",
                             "Category", "Item", "Status"])
        for key, status in results.items():
            category, item = key.split(" > ", 1)
            writer.writerow([timestamp, operator, patient_id, surgery_type,
                             category, item, status])

    print(f"\n  {green('✔')} Log saved → {bold(LOG_FILE)}")


def view_logs():
    """Display recent CSV logs in terminal."""
    if not os.path.isfile(LOG_FILE):
        print(red("  No log file found. Run a checklist first."))
        return

    header()
    print(bold("  📂  RECENT SESSION LOGS (last 20 entries)"))
    separator()

    with open(LOG_FILE, newline="", encoding="utf-8") as f:
        reader = csv.DictReader(f)
        rows = list(reader)

    if not rows:
        print("  Log file is empty.")
        return

    last_20 = rows[-20:]
    fmt = "  {:<20} {:<12} {:<10} {:<10} {}"
    print(fmt.format("Timestamp", "Operator", "Patient", "Status", "Item"))
    separator()
    for row in last_20:
        status = row["Status"]
        colored = green(status) if status == "PASS" else (red(status) if status == "FAIL" else yellow(status))
        print(fmt.format(
            row["Timestamp"][:19],
            row["Operator"][:12],
            row["Patient_ID"][:10],
            colored,
            row["Item"][:45]
        ))
    separator()
    print(f"  Total records in log: {len(rows)}")


# ─────────────────────────────────────────────────────────────────────────────
# MAIN MENU
# ─────────────────────────────────────────────────────────────────────────────

def main():
    while True:
        header()
        print("  1.  Start New Checklist")
        print("  2.  View Session Logs")
        print("  3.  Exit")
        separator()
        choice = input("  Select option [1/2/3]: ").strip()

        if choice == "1":
            header()
            print(bold("  ENTER SESSION DETAILS"))
            separator()
            operator   = input("  Perfusionist / Operator Name : ").strip() or "Unknown"
            patient_id = input("  Patient ID / MRN            : ").strip() or "ANON"
            surgery_type = input("  Surgery Type                : ").strip() or "General"

            results, checked, total, failed_items = run_checklist()
            show_summary(results, checked, total, failed_items, operator, patient_id, surgery_type)
            save_to_csv(results, operator, patient_id, surgery_type)
            input("\n  Press ENTER to return to menu...")

        elif choice == "2":
            view_logs()
            input("\n  Press ENTER to return to menu...")

        elif choice == "3":
            clear()
            print(green("  ✔  Session ended. Stay safe in the OT.\n"))
            break

        else:
            print(red("  Invalid choice. Try again."))


if __name__ == "__main__":
    main()
