Release 6 Notes

Major Changes
* Content generated as complete sentences rather than bag-of-words
* Single topic per document in dataset 1. (Multiple topics per document in dataset 2.)
* Size of user popuation increased to 4000
* Web uploads/downloads added
* Email receipt is replaced by email viewing, to match Vegas data.
* File actions now include "copy", "delete", "open", and "write".
* "Persistent" filesystem and inbox. (Consistency across time and data types.)
* Additional red team scenario


license.txt
* ExactData license information


logon.csv
* Fields: id, date, user, pc, activity (Logon/Logoff)
* Weekends and statutory holidays (but not personal vacations) are included as days when fewer people work.
* No user may log onto a machine where another user is already logged on, unless the first user has locked the screen.
* Logoff requires preceding logon
* A small number of daily logons are intentionally not recorded to simulate dirty data.
* Some logons occur after-hours
  - After-hours logins and after-hours thumb drive usage are intended to be significant.
* Logons precede other PC activity
* Screen unlocks are recorded as logons. Screen locks are not recorded.
* Any particular user’s average habits persist day-to-day
  - Start time (+ a small amount of variance)
  - Length of work day (+ a small amount of variance)
  - After-hours work: some users will logon after-hours, most will not
* Some employees leave the organization:  no new logon activity from the default start time on the day of termination
* 4k users, each with an assigned PC
* 400 shared machines used by some of the users in addition to their assigned PC. These are shared in the sense of a computer lab, not in the sense of a Unix server or Windows Terminal Server.
* Systems administrators with global access privileges are identified by job role "ITAdmin".
* Some users log into another user's dedicated machine from time to time.


device.csv
* Fields: id, date, user, pc, file_tree, activity (connect/disconnect)
* Some users use a thumb drive
* Some connect events may be missing disconnect events, because users can power down machine before removing drive
* Users are assigned a normal/average number of thumb drive uses per day. Deviations from a user's normal usage can be considered significant.
* The file_tree field is a semicolon-delimited list of directories on the device.


http.csv
* Fields: id, date, user, pc, url, activity, content
* Has modular/community structure, but is not correlated with social/email graph.
* Full URLs with paths.
* Words in the URL are usually related to the topic of the web page.
* Content consists of a space-separated list of content keywords.
* Each web page can contain multiple topics.
* Activity can be "WWW Download", "WWW Upload", or "WWW Visit"
* WARNING: Most of the domain names are randomly generated, so some may point to malicious websites. Please exercise caution if visiting any of them.


email.csv
* Fields: id, date, user, pc, to, cc, bcc, from, activity, size, attachments, content
* Driven by underlying friendship and organizational graphs.
* Role (from LDAP) drives the amount of email a user sends per day.
* The vast majority of edges (sender/recipient pairs) are exist because the two users are friends.
* A small number of edges are introduced as noise. A small percentage of the time, a user will email someone randomly.
* Emails can have multiple recipients
* Emails can have a mix of employees and non-employees in dist list
* Non employees use a non-DTAA email addresses; employees use a DTAA email address
* Terminated employees remain in the population, and thus are eligible to be contacted as non-employees
* A friendship graph edge is not implied between the multiple recipients of an email.
* Unlike the previous release, we do not believe the observed email graph follows graph power laws
  because the power-law-conforming friendship graph is overwhelmed by the organizational graph.
* Email size and attachment count are not correlated with each other.
* Email size refers to the number of bytes in the message, not including attachments.
* Content consists of a space-separated list of content keywords.
* "Content" does not specifically refer to the subject or body. We have not made that distinction.
* Each message can contain multiple topics.
* Message topics are chosen based on both sender and recipient topic affinities.
* Activity topic can be Send or View.


file.csv
Fields: id, date, user, pc, filename, activity, to_removable_media, from_removable_media, content
* Each entry indicates activity (open, write, copy or delete) involving a removable media device
* Content consists of a hexadecimal encoded file header followed by a space-separated list of content keywords
* Each file can contain multiple topics.
* File header correlates with filename extension.
* The file header is the same for all MS Office file types.
* Each user has a normal number of file copies per day. Deviation from normal can be considered a significant indicator.

LDAP
* Fields
  - employee_name - First, middle, and last name
  - user_id - Employee user ID
  - email - Employee's work email address
  - role - Job title
  - projects - List of projects (currently can be zero or one project)
* The next 5 are organizational units in descending order of size (i.e., business_unit defines the largest group and supervisor the smallest)
  - business_unit
  - functional_unit
  - department
  - team
  - supervisor
* The project and supervisor fields are known to be missing from the Vegas dataset.

decoy.csv
* A list of decoy files and the hosts on which they reside.
* Filenames are not guaranteed to be globally unique (i.e., two hosts could each have a decoy file with the same name).

psychometric.csv
* Fields: employee_name, user_id, O, C, E, A, N
* Big 5 psychometric score
* See http://en.wikipedia.org/wiki/Big_Five_personality_traits for the definitions of O, C, E, A, N ("Big 5").
* Extroversion score drives the number of connections a user has in the friendship graph.
* Conscientiousness score drives late work arrivals.
* This information would be latent in a real deployment, but is offered here in case it is helpful.
* A latent job satisfaction variable drives behaviors including job searching and punctuality.

Malicious actors
* This data contains five instances of insider threats.

Errata:
* Field Ids are unique within a csv file (logon.csv, device.csv) but may not be globally unique.

