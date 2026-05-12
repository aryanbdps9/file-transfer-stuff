# file-transfer-stuff
This is an AI-generated-with-human-tweaks-repo for local Win11 to Win11. I find myself to need directory-level file transferring tool that's easy to use, while still being customisable. I wanted to be able to just fire up a tool that supports the following workflow:
1. [Sender] Tells me which devices are available in the network
2. [Sender] Lets me pick a directory to send files from
3. [Sender] Shows me which discoverable machines are available and lets me pick one
4. [Receiver] Tells the user that "hey, this file/directory is coming from this device, what to do (decline, else specify the dest dir)

I want to develop an "it just works toolchain" just for me. But I didn't want to pay Github for a private repo. So, there we go!

Here are my priorities for this project. Using a numbered list for labelling, but they don't imply the priority _order_.
1. "It should just work" - no fuss about which port or protocol to use by default
2. "Don't reinvent the wheel" - let's use whatever tools that are already there.
3. "Have trustworthy dependencies" - Pretty self-explanatory. 

I plan to use localsend and build on top of it, just the bare minimum, while upholding the above priorities.

