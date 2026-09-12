# Prompt Engineering Accelerator for AI

## Mastering the Techniques, Patterns, and Strategies Behind High-Performance AI Prompting ##

These instructions will guide you through configuring a GitHub Codespaces environment that you can use to do the labs. 

**1. Change your codespace's default timeout from 30 minutes to longer (60 for half-day sessions, 90 for deep dive sessions).**
To do this, when logged in to GitHub, go to https://github.com/settings/codespaces and scroll down on that page until you see the *Default idle timeout* section. Adjust the value as desired.

![Changing codespace idle timeout value](./images/prompt-accel1.png?raw=true "Changing codespace idle timeout value")

**2. Click on the button below to start a new codespace from this repository.**

Click here ➡️  [![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://codespaces.new/skillrepos/prompt-accel?quickstart=1)

**3. Then click on the option to create a new codespace.**

![Creating new codespace from button](./images/prompt-accel2.png?raw=true "Creating new codespace from button")

This will run for a long time while it gets everything ready.

After the initial startup, it will run a script to setup the python environment and install needed python pieces. This will take several more minutes to run. It will look like this while this is running.

![Final prep](./images/prompt-accel3.png?raw=true "Final prep")

The codespace is ready to use when you see a prompt like the one shown below in its terminal.

![Ready to use](./images/prompt-accel4.png?raw=true "Ready to use")


**4. Open up the *labs.md* file so you can follow along with the labs.**
You can either open it in a separate browser instance or open it in the codespace. 

![Opening labs](./images/prompt-accel23.png?raw=true "Opening labs")

**Now, you are ready for the labs!**


## Troubleshooting

- **Lab 6: `python mcp_server.py` fails with `ModuleNotFoundError: No module named 'mcp.server.fastmcp'`** — your codespace was created before the `fastmcp` version pin was added and installed FastMCP 4.x. Fix it in place with:

```
pip uninstall -y fastmcp fastmcp-slim mcp && pip install "fastmcp>=2.13.0,<3"
```

  Then re-run `python mcp_server.py`. (The uninstall matters: installing over the top leaves 4.x files behind that break `mcp_client_agent.py` even after the server starts.) Codespaces created from the current repo do not hit this.

- **Labs 5-6: the first model call is slow** — `granite4:3b` is loading. Later calls in the same session are fast.
