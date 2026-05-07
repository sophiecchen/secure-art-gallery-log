# Art Gallery Log

This program reads and writes to a secure log, which describes the state of an art gallery: the guests and employees who have entered and left, and people that are in rooms.

The system as a whole must guarantee the privacy and integrity of the log in the presence of an adversary who does not know the authentication token. Without knowledge of the token, an attacker should not be able to:
- Query the logs via logread or otherwise learn facts about the names of guests, employees, room numbers, or times by inspecting the log itself.
- Modify the log via logappend.
- Fool logread or logappend into accepting a bogus file.

---

```
logappend -T <timestamp> -K <token> (-E <employee-name> | -G <guest-name>) (-A | -L) [-R <room-id>] <log>
logappend -B <file>
```
- `-T <timestamp>`: Time the event is recorded (a non-negative integer ranging from 1 to 1073741823 inclusively). Time should always increase.
- `-K <token>`: Token used to authenticate the log. The token is of the form `[a-zA-Z0-9]+`. The user supplies this token via the command line when creating a new log.
- `-E <employee-name>`: Name of employee. Names are of the form `[a-zA-Z]+`. The names can be arbitrarily long.
- `-G <guest-name>`: Name of guest. Names are of the form `[a-zA-Z]+`. The names can be arbitrarily long.
- `-A`: Specify that the current event is an arrival (to the gallery or to a specific room).
- `-L`: Specify that the current event is a departure (from the gallery or from a specific room).
- `-R <room-id>`: Specifies the room ID for an event. Room IDs are non-negative integer characters with no spaces (ranging from 0 to 1073741823 inclusively). Leading zeros in room IDs should be dropped, such that 003, 03, and 3 are all equivalent room IDs.
- `<log>`: The name of the file containing the event log. The log’s filename may be specified with a string of alphanumeric characters (including underscores and periods). If the log does not exist, logappend should create it.
- `-B <file>`: Specifies a batch file of commands.

The full spec for `logappend` can be found [here](https://course.ece.cmu.edu/~ece732/s26/homework/bibifi/logappend.html).

---
```
logread -K <token> -S <log>
logread -K <token> -R (-E <name> | -G <name>) <log>

// OPTIONAL:
logread -K <token> -T (-E <name> | -G <name>) <log>
logread -K <token> -I (-E <name> | -G <name>) [(-E <name> | -G <name>) ...] <log>
```
- `-K <token>`: Token used to authenticate the log.
- `-S`: Print the current state of the log to stdout.
- `-R`: Give a list of all rooms entered by an employee or guest. Output the list of rooms in chronological order. If this argument is specified, either `-E` or `-G` must be specified.
- `-T`: Gives the total time spent in the gallery by an employee or guest. If the employee or guest is still in the gallery, print the time spent so far. Output in an integer on a single line. This feature is optional. If the specified employee or guest does not appear in the gallery, then nothing is printed.
- `-I`: Prints the rooms, as a comma-separated list of room IDs, that were occupied by all the specified employees and guests at the same time over the complete history of the gallery. Room IDs should be printed in ascending numerical order. This feature is optional. If a specified employee or guest does not appear in the gallery, it is ignored. If no room ever contained all of the specified persons, then nothing is printed.
- `-E`: Employee name. May be specified multiple times when used with `-I`.
- `-G`: Guest name. May be specified multiple times when used with `-I`.
- `<log>`: The name of the file log used for recording events. The filename may be specified with a string of alphanumeric characters (including underscores and periods).

The full spec for `logread` can be found [here](https://course.ece.cmu.edu/~ece732/s26/homework/bibifi/logread.html).

---

After creating this program, we reviewed the code of other teams, looking for confidentliaty, integrity, and correctness bugs, or "breaks". We found a number of correctness breaks, as well as an integrity break (see `breaks.json`). We then patched our program given the correctness breaks found against our program (see `fixes`).

---

The information above was taken from the full spec, written by 18-335/732 instructors.

_This project was completed for my secure software systems class and is stored in a private repo as per my university's honor code. If you are a involved in recruitment or hiring and would like details on this project, please contact me._
