---
layout: page
title: User Guide
---

ShutterLink is a **desktop application for managing client contacts and photography engagements, optimized for use through a Command Line Interface (CLI)** while retaining the benefits of a Graphical User Interface (GUI). It keeps each client's contact details together with the type and date of their event.

* Table of Contents
{:toc}

--------------------------------------------------------------------------------------------------------------------

## Quick start

1. Ensure that Java `25` or later is installed on your computer.<br>
   **Mac users:** Ensure you have the precise JDK version prescribed [here](https://se-education.org/guides/tutorials/javaInstallationMac.html).

1. Download the latest `.jar` file from the [ShutterLink releases page](https://github.com/AY2627S1-CS2103T-T08-3/tp/releases).

1. Copy the file to the folder you want to use as the _home folder_ for ShutterLink.

1. Open a terminal, `cd` to the folder containing the JAR file, and run `java -jar addressbook.jar`.<br>
   The ShutterLink GUI should appear in a few seconds with some sample engagement data.

1. Type a command in the command box and press Enter to execute it. For example, type **`help`** and press Enter to open the help window.<br>
   Some example commands you can try:

   * `list` : Lists all contacts.

   * `add n/John Doe p/98765432 e/johnd@example.com t/Wedding d/2026-12-20` : Adds a client engagement for `John Doe`.

   * `delete 3` : Deletes the 3rd contact shown in the current list.

   * `clear` : Deletes all contacts.

   * `exit` : Exits the app.

1. Refer to the [Features](#features) section below for details of each command.

--------------------------------------------------------------------------------------------------------------------

## Features

<div markdown="block" class="alert alert-info">

**:information_source: Notes about the command format:**<br>

* Words in `UPPER_CASE` are the parameters to be supplied by the user.<br>
  For example, in `add n/NAME`, replace `NAME` with a value such as `John Doe`.

* Items in square brackets are optional.<br>
  For example, `edit INDEX [p/PHONE]` can be used as `edit 1 p/91234567` or with another supported field instead.

* Parameters can be in any order.<br>
  For example, if the command specifies `n/NAME p/PHONE`, `p/PHONE n/NAME` is also acceptable.

* Command words are case-sensitive, except for `list`. For example, use `add`, not `ADD`. The `list` command, its `sort/` prefix, and the `date` value are case-insensitive.

* `help` does not accept extra arguments. Extra arguments supplied to `clear` and `exit` are ignored.

* If you are using a PDF version of this document, be careful when copying and pasting commands that span multiple lines as space characters surrounding line-breaks may be omitted when copied over to the application.
</div>

### Viewing help: `help`

Opens a window showing a summary of the available commands.

Format: `help`


### Adding a client engagement: `add`

Adds a client engagement to ShutterLink.

Format: `add n/NAME p/PHONE e/EMAIL t/EVENT_TYPE d/EVENT_DATE`

All five fields are required. Parameters can be supplied in any order, but each prefix can appear only once.

The fields must meet these requirements:

* `NAME` contains only alphanumeric characters and spaces and is not blank.
* `PHONE` contains only digits and is at least three digits long.
* `EMAIL` is a valid email address such as `alex@example.com`.
* `EVENT_TYPE` is not blank and may contain spaces.
* `EVENT_DATE` is a valid calendar date in `YYYY-MM-DD` format, such as `2026-12-20`.

Examples:
* `add n/John Doe p/98765432 e/johnd@example.com t/Wedding d/2026-12-20`
* `add d/2027-01-08 t/Birthday e/betsycrowe@example.com p/1234567 n/Betsy Crowe`

### Listing and sorting contacts: `list`

Use `list` to return to all client engagements after a search or sorted view.
Use `list sort/date` to review all engagements from the earliest event date to the latest.
Past events are included. Engagements on the same date stay in the order they were added.

Formats:
* `list` — all contacts in insertion order.
* `list sort/date` — all contacts by ascending event date.

The heading is `All contacts (N)` or `All contacts by event date (N)`, respectively.
The result panel reports `Showing N contacts.` With no saved contacts, it reports `No contacts to display.`
Here, `N` is the number of displayed contacts.

Examples:
* `list sort/date` brings earlier shoots to the top, including completed dates.
* `list` restores insertion order after sorting.
* `LIST SORT/DATE` also works: the list command, prefix and sort value are case-insensitive.

Each command starts from all records, replacing the previous search or sort.
Displayed indexes are recalculated; subsequent edit/delete commands use the indexes currently shown.
Sorting does not change the saved records or their insertion order. Restarting restores insertion order.

Only the `sort/` prefix and the value `date` are supported. Spaces and tabs may separate arguments,
but the command must occupy one line. Stage filtering is not supported.

| Invalid input | Feedback |
| --- | --- |
| `list sort/name` | `Invalid sort option. Use list sort/date.` |
| `list sort/` | `Empty value for sort/.` |
| `list sort/date sort/date` | `Duplicate parameter: sort/.` |
| `list stage/Booked` | `Unsupported parameter: stage/.` |
| `list 3` or `list sort/date extra` | `Invalid command format. Usage: list [sort/date]` |

Unsupported prefixes are rejected. Invalid list commands leave both the data and current view unchanged.

### Editing a client engagement: `edit`

Edits an existing client engagement.

Format: `edit INDEX [n/NAME] [p/PHONE] [e/EMAIL] [t/EVENT_TYPE] [d/EVENT_DATE]`

* Edits the person at the specified `INDEX`. The index refers to the index number shown in the displayed person list. The index **must be a positive integer** 1, 2, 3, …​
* At least one of the optional fields must be provided.
* Existing values will be updated to the input values.
* The same field validation rules used by `add` apply.
* Each prefix can appear only once.
* After a successful edit, all contacts are displayed in insertion order.

Examples:
* `edit 1 p/91234567 e/johndoe@example.com` changes the phone number and email address of the first displayed engagement.
* `edit 2 t/Corporate d/2027-03-15` changes the event type and event date of the second displayed engagement.

### Finding client engagements: `find`

Finds client engagements whose selected field contains the given search value.

Formats:
* `find n/NAME`
* `find p/PHONE`
* `find e/EMAIL`
* `find t/EVENT_TYPE`

* Exactly one search prefix must be supplied.
* The search is case-insensitive.
* Partial values match. For example, `find n/lex` matches `Alex Yeoh`, and `find t/wed` matches `Wedding`.
* Spaces are part of a multi-word search value. For example, `find n/Alex Yeoh` searches for the substring `Alex Yeoh`.
* Event dates cannot be searched using `find`.
* If there are no matches, ShutterLink reports `No matching contacts found.`

Examples:
* `find n/alex` returns contacts whose names contain `alex`.
* `find p/9123` returns contacts whose phone numbers contain `9123`.
* `find e/@example.com` returns contacts whose email addresses contain `@example.com`.
* `find t/Wedding` returns contacts whose event types contain `Wedding`.

### Deleting a person: `delete`

Deletes the specified person from the address book.

Format: `delete INDEX`

* Deletes the person at the specified `INDEX`.
* The index refers to the index number shown in the displayed person list.
* The index **must be a positive integer** 1, 2, 3, …​

Examples:
* `list` followed by `delete 2` deletes the 2nd person in the address book.
* `find n/Betsy` followed by `delete 1` deletes the first engagement in the search results.

### Clearing all entries: `clear`

Clears all entries from the address book.

Format: `clear`

### Exiting the program: `exit`

Exits the program.

Format: `exit`

### Saving the data

ShutterLink automatically saves data after every successfully executed command. You do not need to save manually.

### Editing the data file

ShutterLink data is saved automatically as a JSON file at `[JAR file location]/data/addressbook.json`. Advanced users may update data directly by editing that file.

<div markdown="span" class="alert alert-warning">:exclamation: **Caution:**
If your changes make the data file invalid, ShutterLink starts with an empty data set at the next run. The invalid file remains on disk until you successfully run a command, which causes ShutterLink to save its current data. We recommend backing up the file before editing it.<br>
Furthermore, certain edits can cause ShutterLink to behave unexpectedly (e.g., if a value is outside its accepted format). Edit the data file only if you are confident that you can update it correctly.
</div>

### Archiving data files `[coming in v2.0]`

_Details coming soon ..._

--------------------------------------------------------------------------------------------------------------------

## FAQ

**Q**: How do I transfer my data to another computer?<br>
**A**: Install the app on the other computer and overwrite the data file it creates with the data file from your previous ShutterLink home folder.

--------------------------------------------------------------------------------------------------------------------

## Known issues

1. **When using multiple screens**, if you move the application to a secondary screen, and later switch to using only the primary screen, the GUI will open off-screen. The remedy is to delete the `preferences.json` file created by the application before running the application again.

--------------------------------------------------------------------------------------------------------------------

## Command summary

Action | Format, Examples
--------|------------------
**Add** | `add n/NAME p/PHONE e/EMAIL t/EVENT_TYPE d/EVENT_DATE`<br>e.g., `add n/James Ho p/22224444 e/jamesho@example.com t/Wedding d/2026-12-20`
**Clear** | `clear`
**Delete** | `delete INDEX`<br> e.g., `delete 3`
**Edit** | `edit INDEX [n/NAME] [p/PHONE] [e/EMAIL] [t/EVENT_TYPE] [d/EVENT_DATE]`<br>e.g., `edit 2 n/James Lee d/2027-01-10`
**Find** | `find n/NAME`, `find p/PHONE`, `find e/EMAIL`, or `find t/EVENT_TYPE`<br>e.g., `find t/Wedding`
**List** | `list` or `list sort/date`
**Help** | `help`
**Exit** | `exit`
