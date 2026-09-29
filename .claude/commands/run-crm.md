---
description: Start the SimpleCRM server in the background and show the local URL
argument-hint: "[port]"
allowed-tools: Bash(python --version), Bash(python3 --version), Bash(py --version), Bash(python app.py*), Bash(python3 app.py*), Bash(py app.py*)
---

Start the SimpleCRM web server for me.

1. Port: use `$ARGUMENTS` if I gave one, otherwise use `8000`.
2. Find the Python command that works on this computer. Try `python3 --version`, then
   `python --version`, then `py --version`. Use the first one that reports Python 3.10 or newer.
3. Run `<python> app.py --port <port>` **as a background process** so this session stays usable.
4. Wait for the line `SimpleCRM is running at ...` and tell me the URL to open in my browser.
5. If the port is already in use, tell me, suggest another port (for example 8080), and ask
   before you retry.

Do not install anything, and do not change any code in this command.
