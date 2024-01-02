Welcome to the livepeer command center, your one stop shop for centrally managing a pool of livepeer orchestrators.

There are two components: the command center and the agent. The command center is the central process that serves the user interface. The agent is a process that runs next to your orchestrator (one agent per orchestrator) and processes messages from the command center.

Command Center:
The command center requires one open port for agent connections. You can also open the http UI port, but it is not advisable as there is no authentication so you need to setup appropriate firewall rules to prevent outsiders from connecting to the UI. Command line flags are used as follows:
- wssPort = websocket port: this is the port used by agents to open a websocket connection to the command center (default is 10000)
- httpPort = http port: this is the port that the UI http server listens on (default is 10001)
- wssHost = websocket host: this is the hostname of the machine running the command center. The websocket server listens on this address (default is localhost)
- httpHost = http host: this is the address that the UI http server listens on (default is localhost)
- password = password is configured on the command center and the agent. The agent's password must match the command center's password for a successful connection.

You can run the command center from the binary as follows:

./orch_cc -wssPort 10000 -wssHost <your-public-ip-address> -httpHost 127.0.0.1 -httpPort 10001 -password yourPassword


Agent:
The agent should run on the same machine as your orchestrator. While this is not required, it does make the setup easier. Command line flags are used as follows:
- serverAddr = command center websocket address
- cliAddr = the livepeer cli address (used by the agent to execute CLI commands and get status of the orchestrator
- password = password that matches the command center password
- nodeID = a unique identifier for your orchestrator. This can be set to any text value, but each orchestrator agent connecting to the command center requires a unique ID. using the same ID for multiple orchs/agents will result in unknown behavior.
- nodeType = "orchestrator" or "transcoder" (currently this field is not actually used, but eventually this tool will be able to manage transcoders as well).
- configFile = the location of your orchestrator config file. This is a required field as the agent writes updates to the config file when config values are modified.

You can run the command center from the binary as follows:

./orch_agent -serverAddr <ip-address-of-command-center> -cliAddr 127.0.0.1:7935 -password yourPassword -nodeID yourNodeID -nodeType orchestrator -configFile ./home/user/.lpData/livepeer.conf


The commands provided above can also be used as the command argument when running the docker image. The image is available at lgdlivepool/livepeer_command_center:v0.1
