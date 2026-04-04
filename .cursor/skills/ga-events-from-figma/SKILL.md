---
name: ga-events-from-figma
description: >-
  Analyzes Figma screenshots or design specs and outputs GA4 event payloads for the
  Redbus taxonomy (ep./epn., load vs click). Use for Figma, GA4, dataLayer, Redbus
  analytics, home/SRP/SL/cust_info screens, or DNI Peru pre-fill flows.
---

# Objective
Your task is to act as a Data/Analytics Engineer. Given a Figma screenshot, design spec, or UI mockup, you must analyze the visual elements and define the exact Google Analytics 4 (GA4) events that should be implemented. You will use the highly-customized Redbus GA4 taxonomy to ensure consistency.

## Step 1: Analyze the User Interface
1. Carefully examine the provided screenshot.
2. Identify all trackable user interactions:
   - Screen/Page generic view (e.g., landing on the page)
   - Clicks on buttons, links, banners, or interactive elements
   - Interactions with form fields (e.g., origin, destination, date pickers)
   - Applying filters or sorting options
   - Selecting a bus or a seat.

## Step 2: Determine the Event Type & Naming Convention
Determine if the interaction triggers a **Screen Load Event** or a **Click/Interaction Event**. 

**CRITICAL RULE - Load vs Click Events:**
- Event names containing **`load`** (e.g., `home_load_event`, `srp_screen_load`, `sl_screen_load`, `cust_info_load`) are triggered **ONLY on the initial load** of that specific page or virtual screen.
- Event names containing **`click`** (e.g., `home_click_event`, `srp_click_event`, `sl_click_event`, `cust_info_click_event`) are used for **EVERY OTHER interaction** on that page. This includes not just literal button clicks, but form field focuses, dropdown selections, applying filters, and toggles.

Use the following mapping to identify the correct `Event Name` (`en`):
- **Home Page**: `home_load_event` (on load), `home_click_event` (any interaction)
- **Search Results (SRP)**: `srp_screen_load` (on load), `srp_click_event` (any interaction), `srp_load_info`
- **Seat Layout (SL)**: `sl_screen_load` (on load), `sl_click_event` (any interaction)
- **Passenger Info**: `cust_info_load` (on load), `cust_info_click_event` (any interaction)
- **Global**: `page_view`, `page_load_time`
- **Experiments**: `ab_exp_sort_bus_score`

## Step 3: Map Screen-Specific Custom Parameters
Redbus utilizes custom event parameters using two namespaces: `ep.` and `epn.`. Both are **key-value pairs** that can carry any type of contextual information for a given event — they are NOT split by data type (string vs number). The prefix simply denotes the parameter namespace used internally by Redbus. **These parameters are tightly mapped to specific screens.** You MUST NOT mix them (e.g., do not use an SRP parameter on the Home page).

- **For all events**, logically inherit standard context parameters: `sid`, `sct`, `seg`, `dl`, `dt`

**Screen Parameter Mapping:**

1. **Home Page Tracking:**
   - `ep.home_clicks`: Describes the element interacted with (e.g., "Source city input", "Search button")
   - `ep.home_values`: Associated value if any
   - `ep.lob`: Line of Business

2. **Search Results Page (SRP) Tracking:**
   - `ep.srp_clicks`: Describes the element interacted with (e.g., "Filter by Bus Operator", "Sort by Price")
   - `ep.srp_values`: Associated value if any
   - `ep.funnel_variant`, `ep.search_type`, `ep.filter_type`, `ep.filter_applied`
   - `epn.doj_doi`: Numeric delta between current date and Date of Journey
   - `epn.bus_count`, `epn.private_count`, `epn.rtc_count`

3. **Seat Layout (SL) Tracking:**
   - `ep.sl_clicks`: Describes the element interacted with (e.g., "Select Seat 4A", "Select Boarding Point")
   - `ep.sl_values`: Associated value if any
   - `ep.bus_operator`, `ep.doj`, `epn.tuple_position`, `epn.available_seats`

4. **Passenger Info (Cust Info) Tracking:**
   - `ep.cust_info_clicks`: Describes the element interacted with
   - `ep.trip_type`, `ep.bus_type`, `epn.bus_ratings`

5. **Cross-Screen Deep Funnel Parameters (SRP, SL, Cust Info):**
   - `ep.source_destination`: (e.g., "Lima_Cusco")
   - `ep.userType`, `ep.signin_status`, `ep.selected_lang`

## Step 4: Generate Configuration Payload
For each identified interaction in the screenshot, output a JSON payload detailing the event exactly as it should be pushed to the `dataLayer` or directly to GA4.

---

# Appendix: Examples by Screen

Below are 5 examples per screen demonstrating the correct application of the load vs click rule and screen-specific parameters.

## 1. Home Page Examples

**Example 1: Initial Page Load**
```json
{
  "UI_Element": "Entire screen",
  "Trigger_Condition": "Home page finishes physical loading",
  "Event_Name": "home_load_event",
  "Parameters": {
    "ep.lob": "Bus_Ticketing"
  }
}
```

**Example 2: Focusing on the Origin Input**
```json
{
  "UI_Element": "Origin City Text Input Field",
  "Trigger_Condition": "User clicks/focuses on the origin field before typing",
  "Event_Name": "home_click_event",
  "Parameters": {
    "ep.home_clicks": "Source Input Focused"
  }
}
```

**Example 3: Selecting a Destination City**
```json
{
  "UI_Element": "Autocomplete Dropdown Items (e.g., 'Cusco')",
  "Trigger_Condition": "User selects a destination city from the autocomplete suggestions",
  "Event_Name": "home_click_event",
  "Parameters": {
    "ep.home_clicks": "Destination City Selected",
    "ep.home_values": "Cusco"
  }
}
```

**Example 4: Selecting the Date of Journey**
```json
{
  "UI_Element": "Date Picker Calendar Day",
  "Trigger_Condition": "User clicks a specific date on the calendar modal",
  "Event_Name": "home_click_event",
  "Parameters": {
    "ep.home_clicks": "Date of Journey Selected",
    "ep.home_values": "31-Mar-2026"
  }
}
```

**Example 5: Submitting the Search**
```json
{
  "UI_Element": "Red 'BUSCAR' Button",
  "Trigger_Condition": "User clicks the search button to find buses",
  "Event_Name": "home_click_event",
  "Parameters": {
    "ep.home_clicks": "Search Button Clicked"
  }
}
```

---

## 2. Search Results Page (SRP) Examples

**Example 1: Initial SRP Load**
```json
{
  "UI_Element": "Entire screen",
  "Trigger_Condition": "Search results finish physical loading",
  "Event_Name": "srp_screen_load",
  "Parameters": {
    "ep.source_destination": "Lima_Cusco",
    "epn.bus_count": 42,
    "epn.doj_doi": 2
  }
}
```

**Example 2: Applying a Bus Operator Filter**
```json
{
  "UI_Element": "Bus Operator Checkbox (e.g., 'Palomino')",
  "Trigger_Condition": "User checks a filter box in the sidebar",
  "Event_Name": "srp_click_event",
  "Parameters": {
    "ep.srp_clicks": "Filter by Bus Operator Applied",
    "ep.srp_values": "Palomino",
    "ep.filter_type": "Bus Operator",
    "ep.source_destination": "Lima_Cusco"
  }
}
```

**Example 3: Sorting the Results**
```json
{
  "UI_Element": "Sort Option 'Precio Máximo'",
  "Trigger_Condition": "User selects sorting by highest price",
  "Event_Name": "srp_click_event",
  "Parameters": {
    "ep.srp_clicks": "Sort Applied",
    "ep.srp_values": "Price Descending",
    "ep.source_destination": "Lima_Cusco"
  }
}
```

**Example 4: Selecting a Bus (Tuple Click)**
```json
{
  "UI_Element": "Bus Result Row (Tuple)",
  "Trigger_Condition": "User clicks a specific bus row to view seats",
  "Event_Name": "srp_click_event",
  "Parameters": {
    "ep.srp_clicks": "Bus Tuple Selected",
    "ep.srp_values": "Palomino - 14:00",
    "ep.source_destination": "Lima_Cusco"
  }
}
```

**Example 5: View Amenities**
```json
{
  "UI_Element": "Amenities Icon/Link on the bus row",
  "Trigger_Condition": "User clicks to see what amenities are provided",
  "Event_Name": "srp_click_event",
  "Parameters": {
    "ep.srp_clicks": "View Amenities Clicked",
    "ep.srp_values": "Wifi, TV",
    "ep.source_destination": "Lima_Cusco"
  }
}
```

---

## 3. Seat Layout (SL) Examples

**Example 1: SL Screen Load**
```json
{
  "UI_Element": "Seat Layout Modal/Screen",
  "Trigger_Condition": "The seat map finishes loading for the selected bus",
  "Event_Name": "sl_screen_load",
  "Parameters": {
    "ep.bus_operator": "Palomino",
    "epn.available_seats": 24,
    "ep.source_destination": "Lima_Cusco"
  }
}
```

**Example 2: Selecting a Seat**
```json
{
  "UI_Element": "Available Seat Icon (e.g., Seat 45)",
  "Trigger_Condition": "User clicks an open seat to reserve it",
  "Event_Name": "sl_click_event",
  "Parameters": {
    "ep.sl_clicks": "Seat Selected",
    "ep.sl_values": "Seat 45 - Lower Deck",
    "ep.bus_operator": "Palomino",
    "ep.source_destination": "Lima_Cusco"
  }
}
```

**Example 3: Selecting a Boarding Point**
```json
{
  "UI_Element": "Boarding Point Radio Button",
  "Trigger_Condition": "User selects where they will board the bus (e.g., La Victoria)",
  "Event_Name": "sl_click_event",
  "Parameters": {
    "ep.sl_clicks": "Boarding Point Selected",
    "ep.sl_values": "La Victoria Terminal",
    "ep.source_destination": "Lima_Cusco"
  }
}
```

**Example 4: Deselecting a Seat**
```json
{
  "UI_Element": "Selected Seat Icon (e.g., Seat 45)",
  "Trigger_Condition": "User clicks a seat they already selected to undo it",
  "Event_Name": "sl_click_event",
  "Parameters": {
    "ep.sl_clicks": "Seat Deselected",
    "ep.sl_values": "Seat 45",
    "ep.source_destination": "Lima_Cusco"
  }
}
```

**Example 5: Proceed to Passenger Info**
```json
{
  "UI_Element": "Button 'Llena los detalles del pasajero'",
  "Trigger_Condition": "User finishes seat/boarding selection and proceeds",
  "Event_Name": "sl_click_event",
  "Parameters": {
    "ep.sl_clicks": "Proceed to Passenger Details Clicked",
    "ep.bus_operator": "Palomino",
    "ep.source_destination": "Lima_Cusco"
  }
}
```

---

## 4. Passenger Info (Cust Info) Examples

**Example 1: Cust Info Screen Load**
```json
{
  "UI_Element": "Form page for Passenger Details",
  "Trigger_Condition": "The passenger form page finishes loading",
  "Event_Name": "cust_info_load",
  "Parameters": {
    "ep.trip_type": "One-Way",
    "ep.bus_type": "Semi-Cama",
    "ep.source_destination": "Lima_Cusco"
  }
}
```

**Example 2: Entering Passenger Name**
```json
{
  "UI_Element": "Full Name Input Field",
  "Trigger_Condition": "User focuses on the name input field",
  "Event_Name": "cust_info_click_event",
  "Parameters": {
    "ep.cust_info_clicks": "Passenger Name Field Focused",
    "ep.source_destination": "Lima_Cusco"
  }
}
```

**Example 3: Selecting Document Type**
```json
{
  "UI_Element": "Document Type Dropdown (DNI, Pasaporte)",
  "Trigger_Condition": "User selects their identification type",
  "Event_Name": "cust_info_click_event",
  "Parameters": {
    "ep.cust_info_clicks": "Document Type Selected",
    "ep.source_destination": "Lima_Cusco"
  }
}
```

**Example 4: Toggling Travel Insurance**
```json
{
  "UI_Element": "Travel Insurance Toggle/Checkbox",
  "Trigger_Condition": "User opts in or out of adding travel insurance",
  "Event_Name": "cust_info_click_event",
  "Parameters": {
    "ep.cust_info_clicks": "Travel Insurance Toggled",
    "ep.source_destination": "Lima_Cusco"
  }
}
```

**Example 5: Proceed to Payment**
```json
{
  "UI_Element": "Continue to Payment Button",
  "Trigger_Condition": "User submits the passenger form to pay",
  "Event_Name": "cust_info_click_event",
  "Parameters": {
    "ep.cust_info_clicks": "Proceed to Payment Clicked",
    "ep.source_destination": "Lima_Cusco",
    "ep.userType": "guest"
  }
}
```

---

## 5. Addendum: DNI-based customer pre-fill (Peru) + passenger/contact UI

**Context:** Aligns the **Passenger details / Contact details** flow (e.g. Figma `fileKey=m81RDrINgrlVlpQJhVTpKM`, `node-id=1-15`) with **Product Requirements Document: DNI-Based Customer Data Pre-fill (Peru)** — DNI-first cust info, tentative DB pre-fill (Phase 1), optional external API + consent (Phase 2), manual fallback, rate limits.

**PII / compliance (mandatory):** Do **not** send the DNI number, full name, or other identifying values in GA. Use **outcome codes**, **booleans**, **field names** (not values), and **aggregated lengths** only where needed. Phase 2 consent text belongs in the product; tracking should record **consent action** without storing the legal copy in `ep.*`.

**Event names:** Still **`cust_info_load`** (once per virtual screen load) and **`cust_info_click_event`** (all other interactions). Use **`ep.cust_info_clicks`** / **`ep.cust_info_values`** for element and safe descriptors; keep **`ep.trip_type`**, **`ep.bus_type`**, **`ep.source_destination`**, **`ep.userType`**, **`ep.signin_status`**, **`ep.selected_lang`** as applicable. Add feature context with **`ep.prefill_flow`** and **`ep.prefill_source`** (string enums below) — these are cust-info scoped, not SRP parameters.

**Suggested enums**

- `ep.prefill_flow`: `dni_first_peru` | `standard_cust_info`
- `ep.prefill_source` (after a lookup resolves): `tentative_db` | `external_api` | `manual_no_match` | `manual_user_choice` | `not_applicable`
- `ep.cust_info_values` for failures: generic codes only, e.g. `invalid_dni_format` | `api_unavailable` | `api_error` | `rate_limited` | `no_tentative_match`

**Example A: Cust info load (DNI-first Peru layout)**

```json
{
  "UI_Element": "Customer information step (restructured: DNI first)",
  "Trigger_Condition": "Passenger/contact step finishes loading for Peru flow with DNI-first UX",
  "Event_Name": "cust_info_load",
  "Parameters": {
    "ep.trip_type": "One-Way",
    "ep.bus_type": "Economy",
    "ep.source_destination": "Origin_Destination",
    "ep.userType": "guest",
    "ep.signin_status": "guest",
    "ep.selected_lang": "es_PE",
    "ep.prefill_flow": "dni_first_peru",
    "ep.lob": "Flight_Ticketing"
  }
}
```

**Example B: Document type = DNI (PRD journey)**

```json
{
  "UI_Element": "ID type selector",
  "Trigger_Condition": "User selects DNI as document type",
  "Event_Name": "cust_info_click_event",
  "Parameters": {
    "ep.cust_info_clicks": "Document Type Selected",
    "ep.cust_info_values": "DNI",
    "ep.prefill_flow": "dni_first_peru",
    "ep.source_destination": "Origin_Destination",
    "ep.userType": "guest"
  }
}
```

**Example C: DNI field focused**

```json
{
  "UI_Element": "DNI number input (8 digits)",
  "Trigger_Condition": "User focuses DNI field",
  "Event_Name": "cust_info_click_event",
  "Parameters": {
    "ep.cust_info_clicks": "DNI Field Focused",
    "ep.prefill_flow": "dni_first_peru",
    "ep.source_destination": "Origin_Destination"
  }
}
```

**Example D: DNI submit — client validation fails (FR-002)**

```json
{
  "UI_Element": "Continue / lookup trigger after DNI entry",
  "Trigger_Condition": "User attempts submit with non–8-digit-numeric DNI",
  "Event_Name": "cust_info_click_event",
  "Parameters": {
    "ep.cust_info_clicks": "DNI Validation Failed",
    "ep.cust_info_values": "invalid_dni_format",
    "ep.prefill_flow": "dni_first_peru",
    "ep.source_destination": "Origin_Destination"
  }
}
```

**Example E: DNI lookup requested (valid format; API/internal call starts — FR-003)**

```json
{
  "UI_Element": "Submit DNI / fetch trigger",
  "Trigger_Condition": "Valid 8-digit DNI; lookup initiated (spinner: Fetching data, please wait)",
  "Event_Name": "cust_info_click_event",
  "Parameters": {
    "ep.cust_info_clicks": "DNI Lookup Submitted",
    "epn.dni_digit_count": 8,
    "ep.prefill_flow": "dni_first_peru",
    "ep.source_destination": "Origin_Destination",
    "ep.userType": "guest"
  }
}
```

*(Optional: omit `epn.dni_digit_count` and send only `ep.dni_format_valid` = true if analytics wants zero digit-related dimensions.)*

**Example F: Pre-fill success (FR-004, FR-005 — fields shown, editable)**

```json
{
  "UI_Element": "Form after successful fetch",
  "Trigger_Condition": "Data returned; name, paternal/maternal, DOB, gender pre-filled and highlighted",
  "Event_Name": "cust_info_click_event",
  "Parameters": {
    "ep.cust_info_clicks": "DNI Prefill Success",
    "ep.prefill_source": "tentative_db",
    "ep.prefill_flow": "dni_first_peru",
    "ep.source_destination": "Origin_Destination"
  }
}
```

*(Phase 2: set `ep.prefill_source` to `external_api` when government/approved API returns data.)*

**Example G: Pre-fill failure — user continues manually (FR-007)**

```json
{
  "UI_Element": "Error banner / full manual form",
  "Trigger_Condition": "Lookup fails; message e.g. We could not fetch your details. Please enter your information manually.",
  "Event_Name": "cust_info_click_event",
  "Parameters": {
    "ep.cust_info_clicks": "DNI Prefill Failed",
    "ep.cust_info_values": "api_unavailable",
    "ep.prefill_source": "manual_no_match",
    "ep.prefill_flow": "dni_first_peru",
    "ep.source_destination": "Origin_Destination"
  }
}
```

**Example H: Rate limit (FR-006)**

```json
{
  "UI_Element": "DNI lookup blocked by rate limit",
  "Trigger_Condition": "Rate limiter rejects further lookups",
  "Event_Name": "cust_info_click_event",
  "Parameters": {
    "ep.cust_info_clicks": "DNI Lookup Rate Limited",
    "ep.cust_info_values": "rate_limited",
    "ep.prefill_flow": "dni_first_peru",
    "ep.source_destination": "Origin_Destination"
  }
}
```

**Example I: User edits a pre-filled field (FR-005)**

```json
{
  "UI_Element": "Any pre-filled field (gray highlight)",
  "Trigger_Condition": "User changes value originally from tentative/API",
  "Event_Name": "cust_info_click_event",
  "Parameters": {
    "ep.cust_info_clicks": "Prefilled Field Edited",
    "ep.cust_info_values": "first_name",
    "ep.prefill_flow": "dni_first_peru",
    "ep.source_destination": "Origin_Destination"
  }
}
```

**Example J: Phase 2 — consent before external fetch**

```json
{
  "UI_Element": "DNI data fetch consent checkbox / CTA",
  "Trigger_Condition": "User acknowledges consent to fetch from external source",
  "Event_Name": "cust_info_click_event",
  "Parameters": {
    "ep.cust_info_clicks": "DNI External Fetch Consent Accepted",
    "ep.prefill_flow": "dni_first_peru",
    "ep.source_destination": "Origin_Destination"
  }
}
```

**Example K: Non-DNI document type (PRD journey — tentative only)**

```json
{
  "UI_Element": "ID type selector",
  "Trigger_Condition": "User selects non-DNI ID; tentative pre-fill path if any",
  "Event_Name": "cust_info_click_event",
  "Parameters": {
    "ep.cust_info_clicks": "Document Type Selected",
    "ep.cust_info_values": "Pasaporte",
    "ep.prefill_flow": "dni_first_peru",
    "ep.source_destination": "Origin_Destination"
  }
}
```

**Example L: Logged-in co-pax / add new passenger (PRD — no UI change when using co-pax)**

```json
{
  "UI_Element": "Add new passenger + DNI path",
  "Trigger_Condition": "Logged-in user adds passenger and enters DNI for lookup",
  "Event_Name": "cust_info_click_event",
  "Parameters": {
    "ep.cust_info_clicks": "DNI Lookup Submitted",
    "epn.dni_digit_count": 8,
    "ep.userType": "logged_in",
    "ep.signin_status": "signed_in",
    "ep.prefill_flow": "dni_first_peru",
    "ep.source_destination": "Origin_Destination"
  }
}
```

**Example M: Save and continue / Pay now (same screen family as Figma)**

```json
{
  "UI_Element": "Save and continue / Continue / Pay now",
  "Trigger_Condition": "User completes passenger + contact and proceeds",
  "Event_Name": "cust_info_click_event",
  "Parameters": {
    "ep.cust_info_clicks": "Save And Continue Clicked",
    "ep.prefill_flow": "dni_first_peru",
    "ep.source_destination": "Origin_Destination",
    "ep.userType": "guest",
    "ep.signin_status": "guest"
  }
}
```

Use **`Proceed to Payment Clicked`** or **`Pay Now Clicked`** in `ep.cust_info_clicks` when the primary CTA label matches the implementation (desktop vs mobile).
