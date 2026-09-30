# Mouse Colony Manager

A single-file colony management tool for small rodent colonies. One HTML file, no install, no server, no account. Open it in a browser, enter your colony, and save the file back to disk. The data lives inside the HTML itself.

This is the blank template. Nothing in this repository contains real animal records.

## Why a single file

Most small labs track colonies in a spreadsheet that nobody validates and that silently drifts out of sync with the animal facility. This tool keeps the spreadsheet's portability (one file, works offline, syncs through any cloud folder) but adds the checks a colony actually needs: cage capacity rules, weaning windows, breeding state, and an event log.

Browser storage is deliberately not used. No localStorage, no IndexedDB, no cookies, no cache. Clearing the browser cannot lose your colony, and there is exactly one source of truth: the file.

## Features

**Colony records**
- Mice with ID, ear punch, strain, genotype loci, sex, DOB and age, cage, status, protocol number, parents, litter, notes
- Cages with barcode, room, biosafety level, cage type, protocol, occupancy
- Rooms with cage capacity and per-cage housing rules, either per-sex maxima or a single total

**Breeding and litters**
- Breeding records with sires, dams, setup date, pregnancy date, end date
- Breeding state inferred automatically: a cage holding both sexes opens a breeding record on its own
- Litters with DOB, pup counts, weaning window (D21 to D28), weaning helper that creates the weaned pups as individual records
- Alerts for weaning due and overdue, sire still in the litter cage, capacity violations

**Strain-level genotype inheritance**
- A line kept homozygous carries one default genotype; per-mouse genotype fields stay blank and inherit it, marked as inherited
- A per-mouse genotype is recorded only where it actually differs
- A tool to collapse genotypes that are redundant with the strain default

**Bulk operations**
- Batch add: generate a run of mouse records across several cages in one pass
- Batch paste: paste tab-separated rows straight from a spreadsheet
- Batch edit: filter by room, strain, sex or status, then set a field across the selection, with a validation pass before anything is written

**Output**
- Excel workbook (.xlsx) with Mice, Genotypes, Cages, Breeding, Litters and Events sheets
- CSV exports for mice, cages and events
- JSON backup and restore
- Cage card printing
- A shareable colony report: static HTML with every style inlined and no scripts, so it survives being pasted into an email body
- A read-only snapshot HTML for sharing a folder link

## Use

Download `index.html` and open it in a browser. Chrome or Edge on desktop gives the best experience, because the File System Access API lets "Save / Replace Working HTML" overwrite your working file in place. In other browsers the save falls back to a normal download.

The working cycle:

1. Open your working HTML
2. Edit the colony
3. Click the save button in the top bar and overwrite the working file
4. Keep that file in a synced cloud folder so the lab shares one copy

The title bar shows unsaved-change state, because in this design closing the tab without saving does lose the edits.

## Keep your working file out of git

If you fork this to track your own colony, remember that animal records, protocol numbers and room numbers usually should not sit in a public repository. Keep the template in git and the working copy out of it. The included `.gitignore` already excludes the exported filenames this app produces.

## Configure

Open the Facilities page and set:

- Database title, shown in the header and in reports
- Default protocol number, applied to new cages and mice
- Rooms, with cage capacity and per-cage housing rules

The template ships with two example rooms, Room A with per-sex limits and Room B with a total limit, purely to show both rule types. Rename or replace them.

## Tech notes

- One HTML file: markup, CSS and JavaScript, no framework, no build step, no external requests except the webfont
- Data is serialised back into the file as a JSON literal when you save, so the saved file is both the app and the database
- The .xlsx writer builds the OOXML package and zips it in the browser, with no library
- Tested in Chrome, Edge and Safari on macOS, and in Safari on iOS for read and light editing

## Limitations

- Single-writer by design. Two people editing two copies at once will diverge; there is no merge.
- No audit trail beyond the event log, and no user accounts. It records what you tell it.
- It is a planning and record-keeping aid, not a regulatory system of record. Your institution's animal facility database remains authoritative.

## Acknowledgments

Built with help from [Claude](https://claude.ai) (Anthropic), which turned the bench-side requirements behind this tool into working code and helped iterate on the data model, the automatic breeding inference, the batch editing flow, and the email-safe report layout.

## License

MIT
