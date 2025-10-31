1. Refer to Development.md to run open hands locally
    - Recommended to set up using mamba
    - Even after `make setup-config` make sure you set up the LLM in the openhands GUI as well
        - use `claude-sonnet-4-5-20250929` provided by anthropic as opposed to OpenHands or any other proxy service
    - set the workspace to the absolute path of a workspace folder, usually in {repo}/workspace
2. Open Openhands GUI
    - Start a conversation without a git repo
    - initially it will take a very long time (since it needs to build a docker image)
    - after the container is spun up, confirm workspace folder is created
    - close the first conversation
    - open workspace, put assets/ and prd.md into the folder. Next create a repo.md file in workspace/.openhands/microagents/repo.md, and place the following prompt in that file

```
<DATABASE>
A PostgreSQL database has been provisioned for this project. Access it using the environment variable `POSTGRES_DATABASE_URL`, which contains the full authenticated connection string to your dedicated database instance.
</DATABASE>

<PORT>
For any web application, use the port specified by the environment variable `$APP_PORT_1`. DO NOT HARD CODE PORT NUMBERS.
</PORT>

<START_SCRIPT>
Create a `./start-server.sh` bash script that serves as the entry point to run your server. This script must:
1. Change to the script's own directory using `cd "$(dirname "$0")"`
2. Execute the server application command
3. Allow all stdin, stdout, and stderr from the server process to flow through naturally without redirection or piping

The script should run the server process in the foreground, allowing direct interaction as if the command was executed manually from the terminal.

Example:
```bash
#!/bin/bash
cd "$(dirname "$0")"
<command_to_run_server>
```
</START_SCRIPT>

<AUTONOMY>
Complete this task autonomously without requesting user assistance. Handle all implementation decisions and requirements independently.
</AUTONOMY>

<TESTING_LABELS>
Add a `data-testid` attribute to all interactive and informational HTML elements using these conventions:

**Naming Pattern:**
- Interactive elements: `{action}-{target}`
  - Examples: `button-submit`, `input-email`, `link-profile`
- Display elements: `{type}-{content}`
  - Examples: `text-username`, `img-avatar`, `status-payment`

**Dynamic Elements:**
For lists, grids, or repeated components, append a unique identifier: `{type}-{description}-{id}`
- Examples: `card-product-${productId}`, `row-user-${index}`, `text-price-${itemId}`
- The dynamic identifier can be any unique value (database ID, array index, unique key)

**Best Practices:**
- Keep test IDs stable and descriptive of element purpose, not appearance or implementation details
- Ensure uniqueness within each group of similar elements
- Apply to all user-interactive elements (buttons, inputs, links, etc.)
- Apply to all elements displaying meaningful information (user data, status messages, dynamic content)
</TESTING_LABELS>

<PRD>
Build every feature to the ***exact*** specification in the PRD below. Do not deviate from requirements or add unprompted features.

<prd.md>
... fill this in
</prd.md>

Note: The PRD content is also available in `./prd.md`.
</PRD>

<PRD_STATUS>
As you are implementing the features, feel free to keep a `prd-status.md` file to help you remember which features have been implemented as you go. This should not just be created as a last step, but rather something that you update as the application is being built. Note that you are *required* to implement everything in the PRD to the exact same user flow without leaving anything else unimplemented or adding additional features.
</PRD_STATUS>

<ASSETS>
If any assets are required to build this application, they can be found in the `assets/` folder.
</ASSETS>
```
3. After the file and the directory is populated, create another session and ensure the other session is closed.
4. Check that the microagent is ready, click it, you should see instructions matching the one we just wrote
5. In that session, tell the agent `Start building the application`
