# Repo Check

**[⬇ Download CTI Repo Check Setup v1_19_1.exe](https://dev.azure.com/CTI-AV/Skunkworks/_apis/git/repositories/Repo-Check/items?path=/dist/CTI%20Repo%20Check%20Setup%20v1_19_1.exe&versionDescriptor.version=main&versionDescriptor.versionType=branch&download=true&%24format=octetStream&api-version=7.1)**
(run it once; it installs for all users, and a newer setup upgrades in place)

The status of every git repository in one folder, at a glance: what needs a
push, what needs a pull, what is not committed, and the remote branch's last
commit and who made it. It re-checks on its own.

**It is read-only on its own.** It runs `git fetch` (which only updates the
remote-tracking branches) and reads status; by itself it never pushes, pulls,
merges, checks out or changes a file. The two exceptions are yours to make:
trusting a folder git refuses, and **Pull** on a row that only needs a simple
pull (both below). Pushing, and any pull with local work in the way, belong
in VS Code or a terminal, where the changes themselves are visible: each row
has an icon that opens the repository there.

The download link above is the `dist\` setup on `main`. Changes to `main`
come in as pull requests, which the maintainer approves.

## Installing

1. Download the setup with the link above and run it. Windows asks for
   administrator rights once.
2. The setup isn't code-signed, so SmartScreen may say *"Windows protected
   your PC"*: choose *More info > Run anyway*. That happens once for each
   new setup, and never for the installed program.
3. **Repo Check** is then in the Start menu, and in *Settings > Apps*, where it
   can be uninstalled.

It needs [Git for Windows](https://git-scm.com). It uses your own git and
your own sign-in (Git Credential Manager) to fetch, and never asks for a
password itself: a repository whose sign-in has expired shows "fetch failed",
and fetching it once in VS Code or a terminal signs it in again.

### Updates

At start-up the program checks whether a newer version exists. If one does,
a banner in the Windows accent colour says so. **Click it to install the new
version**: Repo Check downloads the setup with your own git sign-in (the one
it fetches with), checks it against the SHA-256 the version file gives, and
runs it silently; Windows asks for administrator approval once, and Repo
Check closes, updates and opens again with its settings. A download that
does not match is deleted unrun, and if anything fails the banner offers the
download in the browser instead (as it always does on a development build). To check on demand, click the **···**
button (*More options*) at the top right and choose **Check for updates…**.
The same menu has **Change log…** (what each version brought, newest first,
built into the program from [CHANGELOG.md](CHANGELOG.md)) and **About**. All
three are also in the window's system menu (Alt+Space). The check reads one small file from
`raw.githubusercontent.com`. It sends nothing about the PC and downloads
nothing by itself.

## Using it

**···** > **Help…** shows how to use the program, in the program itself.

1. Choose the folder that holds your repositories (**Browse…**, or type it
   and press Enter). Every repository under it is listed, however deep.
2. That's it. Every repository is re-read on this PC (no network) 30
   seconds after the last check finishes, quietly (the status line keeps
   the summary), and every 5 minutes (the setting beside the toggle) each one is
   fetched first, so new commits on the server show up. **Fetch now** does
   both at once, and also picks up repositories cloned since.

| Column | What it shows |
|---|---|
| (icons) | **Open in Explorer**, **open it in VS Code**, **open it in Azure DevOps**, or for a repo on GitHub the GitHub icon, which **opens it on GitHub**, or for one on GitLab (gitlab.com, or a self-hosted server named `gitlab.*`) the GitLab icon, which **opens it on GitLab**, or for one on Bitbucket (bitbucket.org, or a Bitbucket Server named `bitbucket.*`) the Bitbucket icon, which **opens it on Bitbucket**. One site icon per row, whichever host the remote is on. With none of those remotes that icon is dimmed and its tooltip says why. The SharePoint icon **opens the job's Yoda folder**: the README's link whose text is a job number (`[J26170078](https://conferencetechinc.sharepoint.com/...)`, as in Job/ST History), or a bare SharePoint address with the job number as one of its folders (`https://conferencetechinc.sharepoint.com/.../J22110067?...` on a line of its own); with several, the most recent job (the later year, then the higher job count). A link in code (an inline span or a fenced block) is an example and does not count. A README with no such link dims it; the README is re-read when it changes. Each icon's tooltip names what it opens and, below that, the folder path or link (the Yoda one names the job number too). Double-click or Enter on a row also opens VS Code. |
| Job Number | The job number of that Yoda link. **Fix README** when the README's newest job-number link does not go to Yoda (an unfilled `[J26190119](paste_link_to_yoda_here)`): the SharePoint icon is then dimmed, even when an older job has a Yoda link, so it never opens the wrong job's folders, and its tooltip says why. A job number in plain text does not count. Blank when the README links no job (the dimmed SharePoint icon's tooltip says these come from README.md) |
| Project | The Azure DevOps project (the GitHub owner; the GitLab group path, `group/subgroup`; the Bitbucket workspace, or a Bitbucket Server project key); for a repository with no remote, the folder it sits in |
| Repo Name | The remote's repository name; for one with no remote, its folder |
| Service Ticket | The service tickets (`ST` and nine digits) the README links to Yoda, the same two ways as a job: `[ST262300462](https://conferencetechinc.sharepoint.com/.../Service/ST262300462)`, or a bare SharePoint address with the ticket as one of its folders. Every one counts (tickets do not replace each other as jobs do): one shows its number, several show **Multiple (n)**. The right-click menu opens each: one ticket has its own item, several are listed alphabetically under **Service Tickets**. With no job, the SharePoint icon opens the one ticket's folder, or with several lists them to choose from. Export CSV gives them all, separated by semicolons. A ticket link that is not to Yoda does not count. A removed repository keeps them |
| Branch | The branch checked out; **(detached at 1a2b3c4)** when none is. A removed repository keeps the last one seen this session. The name is in the attention colour (as Pull and Push) when it is not the remote's default branch: what the remote's HEAD names as last fetched, else `main`, else `master` (a HEAD naming a deleted branch is not trusted); with no default known (no remote), it stays grey. Export CSV gives the default as "Default Branch" |
| Status | **Up to date**, **Push (3 commits)**, **Pull (2 commits)** or **Both (push 3 commits, pull 2 commits)** against the branch it tracks, as of the last fetch. Files not yet committed (changed and new together) count as work to push: **Push (5 files)**, **Push (3 commits, 5 files)**, **Both (push 3 commits and 5 files, pull 2 commits)**. Also: No upstream branch, Upstream branch gone, No remote, Detached HEAD, No commits yet, each with **(n files)** when there are any, Untrusted, Error, and "- fetch failed" when the last fetch did not work (the count is then as of the fetch before). There is no Uncommitted column since 1.19.0: Status says the same |
| Size on Disk | The space the repository's folder takes, `.git` included, counted as Explorer's *Size on disk* (junctions are not followed). Measured on the first check and on each fetch, otherwise at most every 10 minutes. Sorts largest first; Export CSV gives it in bytes. Blank for a removed repository |
| Last Commit Date / User / Message | The newest commit on the remote branch this one tracks, and its subject line |

Which repository a row is (Job Number, Service Ticket, Project, Repo Name) shows in the main text colour, CTI navy in light mode; the rest of the row is grey, so the names read first. Status keeps its colours. A removed repository's row is a lighter grey throughout.

**Closing the window keeps Repo Check running in the system tray**, still
checking. Click the tray icon (or open Repo Check again) to bring the window
back; right-click it for **Fetch now** and **Exit**. The first time, a
notification says where it went. Windows 11 puts every new tray icon in the
hidden overflow (the **^**), and no program can move itself out: to keep the
icon and its number in view, turn **Repo Check** on in *Settings >
Personalization > Taskbar > Other system tray icons*.

**A number shows where Repo Check is**: on the taskbar button while the
window is open, on the tray icon while it is hidden. By default it counts the
repositories to pull (Pull or Both); the **···** menu's **Taskbar badge**
switches it to the repositories to push (Push or Both), to push or pull
(Pull, Push or Both, each counted once), or turns it off. It
disappears at 0, and shows "99+" past 99.

The **···** menu's **Columns** shows or hides each column (right-clicking a
column header shows the same list, as in Explorer); all start shown, and
**Repo Name** always shows (its item is checked and greyed). A hidden
column keeps its width for when it comes back, and the choice is remembered.
Drag a column's header to move it; the icons stay first. The order is
remembered, and **Reset column order** at the bottom of the same list puts
every column back.

The **···** menu's **Theme** picks **System** (the default: Windows' own light
or dark mode, followed as it changes), **Light** or **Dark**. The choice
applies at once and is remembered.

Click the **Log** header to collapse the log to just that header;
Repositories then takes the space. It stays that way until you open it again.
Beside the collapsed header a counter shows what was logged since, errors and
warnings named (*"12 new lines, 1 warning"*); opening the card resets it.

Click a column header to sort (again to reverse); sorting by Status puts
what needs attention first. Right-click a row (or Shift+F10) for the same
four openers, **Open Parent
Folder** (Explorer one level up, the repo's folder selected), **Copy the folder path**, **Copy the Azure DevOps link** (or
**Copy the GitHub link** / **Copy the GitLab link** / **Copy the Bitbucket link**), **Copy the Yoda link**, **Copy git's message** and, on an
Untrusted row, the two trust choices.

**Pull (n)** is on the same menu, enabled only when a simple pull can
happen: the row's Status is exactly Pull (n) -- commits to pull, none to
push, and no changed or new files (with any of those the row is Both, and
Pull is greyed). After a Yes it asks git again (the row can be 30 seconds
old) and fast-forwards the branch to the remote branch as last fetched, which
is what the row showed; it does not reach the network. If anything changed
in the meantime it does nothing and the log says why. Changes
and problems are written to the log, for example *"Skunkworks / Repo-Check: Pull
(2)"*. **Export CSV…** saves the list as shown.

**Untrusted** means git refuses the folder because another Windows account
owns it ("dubious ownership"; common for folders an administrator created).
When a check finds new ones, it asks about each in turn ("Repository 2 of
5"), offering:

- **Trust this folder**: added to git's `safe.directory` list
  (`git config --global --add safe.directory "<folder>"`), exactly as git
  suggests;
- **Trust everything in "<parent>"**: the folder one level up is added as
  `"<parent>/*"`, which trusts every repository under it, at any depth,
  including ones cloned there later (git 2.46 or later; a drive's root is
  never offered);
- **Skip this one**: it asks again the next time Repo Check opens.

While more repositories are waiting, the question also has an **Apply to
all** checkbox: tick it and the answer you press is given to every remaining
repository in the round without asking again (each one's own folder, or
everything one level up from each one, or skip them all). The log says so.

Before each question (and before each repeated answer) it asks git whether an
earlier answer already covers that repository (trusting a folder one level up
usually covers its neighbours); if so, it is not asked about, and the log says
"Already covered". Closing the question stops asking for this session.

The same two choices are on an Untrusted row's right-click menu. Repo Check
never trusts a folder without that click, and only trust folders that are
yours.

What is searched: every folder under the chosen one, up to 8 levels deep.
A repository's own subfolders are not searched (so a repository inside
another is not listed separately), and `node_modules`, folders starting with
`$` and links (junctions) are skipped.

**Delete local…** is on the same menu, enabled on Up to date and Pull rows:
the local copy goes to the Recycle Bin, only when nothing would be lost. It
asks git first, and again after the Yes, and refuses -- saying why -- if the
folder holds anything the remote does not: uncommitted or new files, commits
to push on any branch, a branch only on this PC (or gone from the remote), or
a stash. The question says what happens: the folder goes to the Recycle Bin,
the copy on Azure DevOps (or GitHub, GitLab, Bitbucket) is not touched, and
the row stays under Show removed, where the download icon clones it again.
Files git ignores (build output, local settings) do not stop it; the question
lists them, one per line (a whole ignored folder is one line; past 15, "...and
n more"), since they go with the folder.

If that leaves its folder empty (and that one's folder, and so on -- never the
watched folder), it then offers to recycle the highest empty one, which takes
the empty ones inside it; a No offers the next one down.

**Show history** (same menu) puts that repository's commits in the list's
place: its branch and the branch it tracks, as one timeline, newest first
(the last 500), with each commit's date, author, message and short id.
Commits only on the remote say **To pull**; only here, **To push**; shared
ones say nothing. The card's title names the repository and branch; **Back**
(or Esc) returns to the list. It is read once when opened (the remote branch
as last fetched): no network, nothing polled.

### Removed repositories

Repo Check remembers every repository it has listed: its folder, remote,
names, links, job number and last commit, as last seen. That is what the
list shows the moment it opens, before git has answered (each row then fills
in), and what is left of a repository after its folder is gone -- deleted
from Explorer, by a script, or in a whole job folder, while Repo Check was
running or not.

**Show removed** (beside Export CSV…) lists those too: greyed, after the
repositories on disk, with Status *Removed* and the date it was found gone.
In the folder's place is a green download icon: **Clone again** (dimmed when
the repository had no remote). The VS Code icon is off; the Azure DevOps (or
GitHub, GitLab, Bitbucket) and Yoda icons still work, so an old job is two
clicks from its repo and its Yoda folder. Double-click opens the repo's web page. A removed
row's right-click menu has:

- **Clone again…**: asks first, with the folder it was in filled in (edit it,
  or **Browse…** for where the folder goes); it refuses a folder that already
  holds files. It clones with your own git sign-in, without a prompt: if git
  has no sign-in for that remote, it fails and the log says so (clone it once
  from VS Code). The log says when it is done; a clone outside the watched
  folder is not listed.
- the web and Yoda openers and **Copy** items, as on any row;
- **Remove from history**: forgets it. Nothing on disk or on the remote is
  touched.

A removed repository that is back at its folder is read again ("Back on
disk"); one whose remote is on disk at another folder (cloned again, or
moved) is dropped from the removed ones. If the watched folder itself is
missing (a drive not connected), nothing is marked removed. Removed
repositories are never read by git, so a long history costs nothing between
checks.

## Settings file

Settings live in `CTI-RepoCheck.ini` in `%LOCALAPPDATA%\CTI-RepoCheck\`, one per
Windows user. (An exe run from outside Program Files, such as a development
build, keeps the file next to itself instead, unless that folder is
read-only.) The log's first line shows which file is in use.

- `[Settings]`: `Folder`, `AutoFetch` (1/0), `FetchMinutes` (1-1440),
  `Badge` (`pull`, `push`, `both` or `off`; default `pull`),
  `CheckUpdates` (default 1; 0 skips the start-up check; the menu item still
  works).
- `[Window]`: position, column widths, the sort, `LogCollapsed` and `ShowRemoved`.
- `TrayHintShown` (in `[Settings]`) records that the tray notification has been shown.

The remembered repositories are in `CTI-RepoCheck-History.txt` beside it (UTF-8,
one repository per line, tab-separated); a repository outside the folder being
watched is kept there for when that folder is watched again.

## Building

Requires MSYS2 with the UCRT64 gcc toolchain
(`pacman -S mingw-w64-ucrt-x86_64-gcc`) and, for the setup, Inno Setup 6
(`winget install JRSoftware.InnoSetup`). Then run:

```powershell
.\build.ps1
```

This produces `build\Repo Check.exe` and, from it,
`dist\CTI Repo Check Setup v<version>.exe`, e.g. `v1_0_0` (`installer\Repo Check.iss`;
the setup's name carries the version, with underscores). `dist\` holds only
the setup, which is what is committed; the previous version's is removed.
Without Inno Setup the exe is still built, so the tests can run.
`.\build.ps1 -Out <path>` writes the exe elsewhere and skips the setup. The
exe is x64, statically linked, and imports only Windows system DLLs.

**The version** lives in `src\resource.h`: `APP_VERSION_STR` and
`APP_VERSION_NUM` change together. The installed exe's name never carries
it (`Repo Check.exe`); the setup's name does.

**Releasing a version:**

1. Bump both version defines and update the download link at the top of
   this README to the new setup's name (it appears twice in that line).
   Add the version's entry at the top of [CHANGELOG.md](CHANGELOG.md): `## <version> -- <date>`,
   then a `- ` line for each thing a user gains. The program shows that file
   rendered, and knows only `#`, `##`, `- ` and `**bold**`.
   Run `.\build.ps1`; it warns if the link does not name the setup it just
   built, if the change log's first heading is not this version, or if it
   uses other markdown. Commit the setup with the change and merge the pull request into
   `main`.
2. **Right after that**, replace `version.json` with the line `build.ps1` printed (it carries the
   setup's `sha256`; without it, installed copies fall back to the browser download), in the public repo
   [logan-vaughn-cti/repo-check-version-check](https://github.com/logan-vaughn-cti/repo-check-version-check)
   (the GitHub web editor is enough): the new `version`, and the new
   download link as `url`. Not before the merge (users would be sent to a
   setup that is not there yet), and not much after (the merge removes the
   previous setup, so the old `url` stops working).

```json
{"version": "1.9.0", "url": "<the download link above>", "sha256": "<the setup's SHA-256>", "notes": "One line about what changed."}
```

The program downloads the setup from this repo itself (`APP_DOWNLOAD_URL`), never from the
version file's `url`: the public file can only say which version, and which hash to insist on.

`version` must be digits and dots. `url` is what the banner opens, and
must start with `https://`; without one, the program opens this repo's
page instead (`APP_DOWNLOAD_URL`). `notes` is optional. The repo holds only
that file and a README: no source and no exe. The address is `UPDATE_URL`
in `src\resource.h`.

### Icon

`assets\make_icon.ps1` builds `assets\app.ico`: the CTI triangle mark
(`assets\cti_mark.png`) bottom-right and a repository badge (a branch glyph,
drawn by the script) top-left, under the mark where the two meet. The badge
is top-left because Windows draws the taskbar number bottom-right; the mark
is moved, never flipped. `cti_logo.png`, the window header, and
`cti_mark.png` come from CTI's brand logo files (the same files DHCP Server
uses). `azure_devops.ico` is the Azure DevOps site icon and `sharepoint.ico`
the SharePoint site icon (both Microsoft artwork; fine for a CTI-internal tool).

### Look

The same Windows 11 design as DHCP Server: light and dark follow the Windows
app setting, live; buttons and highlights use the Windows accent colour; the
font is Segoe UI Variable (Segoe UI on Windows 10). It all lives in
`src\theme.c`.

## Testing

```powershell
python tests\repo_status_test.py
```

This builds a scratch folder of real repositories with a bare remote on disk
(no network), runs the real exe on it with `--report`, and checks the CSV
after each run: every status with its commit and file counts, the move of settings saved with the old Uncommitted column, the remote branch's
last commit, Azure DevOps / visualstudio.com / ssh / GitHub / GitLab / Bitbucket addresses,
nested and skipped folders, a failed fetch, that `--no-fetch` really does not
fetch, and that **nothing a person works on changes** (HEAD, branch, working
tree, index and stash of every repository are compared before and after).
It also runs the history through a series of runs: a repository deleted outside
the app becomes Removed and keeps what was last seen, Show removed off hides
it, a missing watched folder marks nothing removed, another folder's entries
are kept, one cloned back is read again, and one moved is dropped.
The window appears for a few seconds per run; leave it alone.

```powershell
python tests\change_log_test.py
```

"..." > Change log, opened through the system menu: the title, the built
version first, every version heading in order, a bullet for every `- ` line
and no markdown marker showing; the text follows a resize, the window never
shrinks below its first size, and Close closes it. The windows appear for a
few seconds (no mouse); it needs any running copy Exited.

```powershell
python tests\delete_test.py
```

Delete local on the real window: greyed on a Push row; refused, saying why,
for a stash, a branch only on this PC and a file added while the question is
up; a real delete whose question names the remote and counts ignored files;
the empty folders it leaves offered highest first, a No falling to the next;
the watched folder never offered. It checks the folders reached the Recycle
Bin (by their original location), then removes this test's items from it. It
moves the mouse for about half a minute (hands off) and needs any running
copy Exited.

```powershell
python tests\history_view_test.py
```

Show history on the real window: five commits with set dates (two shared, two
only on the remote, one only here) listed newest first and marked To pull /
To push; the title with the branch, or "detached HEAD"; Back and Esc. It moves
the mouse for about half a minute (hands off) and needs any running copy
Exited.

```powershell
python tests\history_test.py
```

Removed rows on the real window: the Show removed switch, a removed row's own
menu, Clone again (the old folder prefilled, a folder holding files refused,
then a real clone from a remote on disk) and Remove from history. It moves the
mouse for about half a minute (hands off) and needs any running copy Exited.

```powershell
python tests\pull_test.py
```

The row menu's Pull, clicked for real on local repositories (bare remotes on
disk, `--no-fetch`): offered only on a plain Pull row and greyed on Both and
the rest, No changes nothing, Yes fast-forwards, and a file added while the
menu is open makes it refuse. It moves the mouse for about half a minute
(hands off) and needs any running copy Exited.

| Switch | Effect |
|---|---|
| `--folder <path>` | watch this folder (instead of the saved one) |
| `--report <file>` | after the first full cycle, write the CSV there and close |
| `--no-fetch` | never fetch; local reads only |
| `--ini <file>` | use this settings file |
| `--log <file>` | also write the log to a file |
| `--theme light` / `--theme dark` | force the light or dark look (screenshots) |
| `--update-url <url>` | read the version file from this address instead |
| `--no-update-check` | skip the start-up update check |
| `--update-repo <url>` | download the setup from this repo instead (tests use a bare repo on disk) |
| `--self-update-test` | take an update without asking, as an installed copy would, but only log the setup's command; then close |
| `--code-path <exe>` | start this program in Code.exe's place for Open in VS Code (tests) |

## Source layout

| Path | What |
|---|---|
| `src/repo_status.c` | finding repositories, running git (no window, no prompts, no index lock), reading each one's status, parsing remote addresses |
| `src/repo_history.c` | the remembered repositories: the list at start-up, and the removed ones |
| `src/main_window.c`, `src/app.rc` | the window, the background cycle, the list, export, the update check, Clone again |
| `src/taskbar_badge.c` | the number on the taskbar button (an overlay icon drawn at run time) |
| `src/theme.c` | the Windows 11 look, the list's colours and Open icons |
| `installer/Repo Check.iss` | the setup (Inno Setup 6) |
| `tests/repo_status_test.py` | the end-to-end test |
| `src/self_update.c` | installing an update from the banner: git download, SHA-256 check, silent setup |
| `tests/self_update_test.py` | that path end to end against a stand-in repo and version file (`--self-update-test` stops before installing) |
| `tests/history_view_test.py` | Show history on the real window: order, To pull / To push, title, Back and Esc (needs no other copy running) |
| `tests/delete_test.py` | Delete local on the real window: the refusals, the question, the Recycle Bin, the empty folders (needs no other copy running) |
| `tests/history_test.py` | removed rows on the real window: Show removed, Clone again, Remove from history (needs no other copy running) |
| `tests/tickets_test.py` | several service tickets on the real window: the column, the row menu's items and submenu, the Yoda icon's list (no mouse; needs no other copy running) |
| `tests/open_code_test.py` | Open in VS Code gets the user's own environment, even when Repo Check was started from a VS Code terminal (no mouse; needs no other copy running) |
| `tests/focus_ring_test.py` | a click hides the keyboard-focus ring and Tab brings it back (no mouse, no keyboard; needs no other copy running) |
| `tests/tray_test.py` | the close button hides to the tray, a second launch brings the window back, a session end really exits (needs no other copy running) |

## License

Copyright (c) 2026 Conference Technologies, Inc. All rights reserved
(`LICENSE.md`): a CTI tool, for use within CTI.
