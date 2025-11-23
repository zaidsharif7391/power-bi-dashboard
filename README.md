# power-bi-dashboard
Objectives

•	Build a Power BI semantic model and dashboards for:
•	Warehouse stock movements and balances from WH Stock Mastersheet and In-ward Register.
•	Hotel Load modification status and regional updates (Total summary, detail sheets, region pivot summaries).
•	Warranty case pipeline (lodged, open, closures, RCA/CRN status, zone color buckets).
•	Debtors comparative summary and working schedules (aging, deduction categories, reconciliation flags).

Data sources and scope

•	Warehouse and logistics:
•	MIS-As-on-31-July-25.xlsx, “WH Stock Mastersheet”: Goods receipt/despatch lines with SAP codes, part descriptions, bin/box locations, and balance tracking by rack/box.
•	“In-ward Register”: sourcing, PO alignment, unit prices, LR/courier references, and value roll-ups.
•	Hotel Load program:
•	Electric-Hotel-load-modification-status_Master-1.xlsx: Total summary by shed, granular Detail1 records (per loco, HLC-A/B), region summaries (Hyderabad), and “Updation Date” by region.
•	Warranty governance:
•	Warranty-Data.xlsx: Summary KPIs (zone color coding by days), detailed case register with CRN stage progress, RCA dates, values, and closure notes.
•	Receivables and deductions:
•	Debtor-Report-May-25.XLSX: Debtors Comparative Summary and working schedule (aging buckets, TDS/GST/EMD, warranty deductions, R Note status).
•	akriti-1.csv: Line-by-line receivable/deduction and categorization (Not due/Overdue/Old legacy) for CR, CLW, WR, NR, etc.

Methodology

5.1 Data ingestion and cleaning
•	Power Query steps:
•	Standardized headers; normalized date types; trimmed/cleaned text; split multi-line cells (e.g., combined vendor/PO strings) to atomic fields; deduplicated and enforced composite keys for inventory documents.
•	Conformed categorical encodings like Region (BAN/CHE/DEL/HYD/KOL/MUM) across Total summary and detail sheets.
•	Warranty and debtors: mapped “Zone colors” by days open; separated TDS/GST/EMD/Liquidated Damages/Warranty deduction drivers; built a dictionary for status.

5.2 Data modeling (Power BI)
•	Star-schema design for:
•	Fact tables: Stock movements (receipt/dispatch), Hotel Load actions (by shed/loco/HLC), Warranty cases (by WRA number), AR line-items.
•	Dimensions: Date, Region, Shed, Customer, Product, Project (WBS), and Document types (GST/IT TDS, EMD, Warranty).
•	Role-playing dates for received/dispatch dates, warranty event dates, CRN date, and receivable due dates.
5.3 DAX measures (representative)
•	Receivable Aging KPIs:
•	Total Overdue = SUMX(Overdue Lines, [Amount LC]) filtered by Aging buckets.
•	Warranty Deduction Total = SUM of lines tagged Warranty across summaries and working file.
•	Warehouse Inventory:
•	Balance Qty by Part = SUM(Received) – SUM(Dispatched), controlled by rack/bin filters.
•	Hotel Load Conversions:
•	Input Choke Change Total/Share by Region = SUM([Input choke change]) and % of grand total (BAN/CHE/DEL/HYD/KOL/MUM).
•	Warranty Pipeline:
•	Open vs Closed Cases; Zone distribution counts (Red/Orange/Yellow/Green/Blue) using date diffs.
5.4 Visual design (Power BI)
•	Executive overview pages for each domain with KPI cards, trend lines, donut share by region/customer, and navigation to detail layers.
•	Drill-throughs from project tiles to WBS/line items (e.g., CLW_HLC_22x; PLW_6K_44x; WR-30x 2x130) showing deductions and status lineage.
•	Region/shed slicers aligned to hotel load metrics; stock filters by rack/box for pick-lists.
Work products

6.1 Warehouse BI (Inventory movements)
•	From “WH Stock Mastersheet”, constructed stock receipt/dispatch ledger views with SAP/Part/Description/UOM, linked to bin/rack for location accuracy. Balance Qty validated using staged dispatches (e.g., HARTING Hood 09300240581 progressive dispatch across multiple UP/DC vouchers and balance decrements).
•	In-ward register value roll-ups (Unit Price × Received Qty), LR/courier tagging for logistics analytics, and PO-level aggregation.
6.2 Hotel Load modifications BI
•	“Total summary” metrics for sheds across tasks: Software version updates, fuse replacements, wire shifting, sealing tasks, Input choke change, display button/DC link supporter, etc., with region assignment (BAN, CHE, DEL, HYD, KOL, MUM).
•	“Hyderabad Summary” pivot validated counts by BZA/Lallaguda and grand totals (e.g., 172 panels; specific action counts) for correctness.
•	“Detail1” provides per-loco HLC-A/B entries with modification flags (“Conv-A DONE/Conv-B DONE”), serial numbers, and dates—wired into drill pages to audit specific interventions.
6.3 Warranty dashboard
•	“Summary” table: Aggregated counts (Total Lodged/Open/Closed), color zoning by days, product breakdown (Propulsion Set/Hotel Load/9000 HP/SIV/TM), and CRN status—presented as top-level KPIs.
•	Detailed page: CRN stage 1/2 dates, RCA submission flags, closure and claim values (e.g., 20532000 recurring nominal), with remarks (e.g., “RCA Pending with Nashik,” “Material will dispatch,” “Under observation”).
•	Filters by Shed Name (e.g., WCR/NKJ, SCR/LGD, CR/AJNI, WR/BRC, etc.) and time windows (WRA Date/Expiry).
6.4 Receivables/AR dashboard
•	“Debtors Comparative Summary” tracks Not Due vs Overdue month-over-month (e.g., Not Due ≈ 803.86 Cr; Overdue ≈ 399.50 Cr at 30.05.2025), shows drivers like Warranty, R Notes, Old Legacy, and GST/IT TDS.
•	“Working File for DS - April” exposes line-level due dates, aging buckets, and tags (Warranty, PVC, LD, EMD, Others), enabling accurate bucketing and action planning.
•	“akriti-1.csv” supports CR/CLW/WR line matching with categorization (Not due/Overdue/Old legacy) and references (e.g., TDS, GST TDS, Retention, EMD), compressing audit efforts and reconciliation flows.

Results and insights

•	Hotel load program:
•	Region contribution to Input choke changes computed (e.g., shares derived in region_stats; Unknown encapsulates rolled totals or header artifacts requiring cleaning rules). This fed a Region vs Shed contribution view for targeting resource allocation.
•	Warehouse operations:
•	Receipts and progressive dispatches align with balance decrements; bin-box mapping aids physical verification and cycle counts; inward register captures value exposure by vendor/place and transporter.
•	Warranty governance:
•	High-frequency claim value bands (≈ 20.53M nominal) across Propulsion/Hotel Load categories; color buckets emphasize timeliness and closure focus; RCA/CRN lags ascertain attention areas.
•	Debtors control:
•	May vs April swing in Overdue (+46.95 Cr) with segmentation by Warranty, R Notes, Old Legacy.
