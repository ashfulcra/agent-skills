# fulcra-workspaces

Gives several agents one place to work instead of several places to work alone.

Run more than one agent and they immediately need things people on a team take for granted: somewhere shared to keep notes, a way to leave each other messages, and a place to put the artifacts they produce for you.

A workspace provides those. Shared memory both agents read and write, a team inbox so work can be directed at a particular agent rather than shouted into a session, and user artifacts — the actual outputs — kept where you can get at them without going through whichever agent made them.

It is built on Fulcra's versioned file storage, so nothing is silently overwritten. When two agents touch the same thing, the earlier version is still there.

For presence, roles, handoffs and reviews on top of this, see `fulcra-agent-coordination` in fulcradynamics/community-skills.
