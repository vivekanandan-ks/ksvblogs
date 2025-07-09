Nix-(AaC) Anything as Code:
Shells Packages Containers Systems & More - All Reproducible Declarative & Reliable


1) Modern Chaos:

Throughout history, we've solved countless challenges through evolving paradigms, innovative tools, and strategies. 
Now we're in the AI era 🤖, yet we still struggle with a decades-old problem:

I used to keep a README 📄 full of setup steps.
Then I had a Makefile 🛠️. Then a Dockerfile 🐳.
Then a setup.sh, a requirements.txt 📦, and a ci.yml 🤖.
And still… things broke. 💣
Someone would clone the repo and ask:
"What version of Python does this need? 🐍"
"Do I install that with Brew 🍺 or Pipenv 📦?"
"Why doesn't the Docker build work on my machine? 💻"

In today's fragmented tooling landscape 🌐, projects rely on multiple moving parts:
Language-specific package managers
Container runtimes
System setup scripts
CI configurations, etc

Even in the age of AI, we still face these same issues. Why? The reason for all these are the follows:

1.1) Divergent System (Known state ➡️ Unknown state)🧩

❌ Everything is Imperative 📜

We instruct how to get things done, instead of declaring what we want. 
Like asking someone to make pasta 🍝-everyone does it differently even if ingredients are the same. 
Very careful recipe might work for pasta, but not for the software deployments where minute difference leads to inconsistencies and failures.
We've become accustomed to this behavior through repeated troubleshooting and temporary fixes. 
But then, as the modern and future practices evolve and scale, this approach makes the hell seep through these small cracks and make portable VMs, container or any type of deployment a painful process requiring frequent or occasional intervention costing budget, productivity and the overall reliability of the deployment pipeline😓.

✅ Solution: Declarative Approach🧾

Moving Towards Declarative Systems

A significant improvement came with the adoption of config files over traditional scripts. 
This shift elegantly solved the imperative challenge by introducing a Declarative ✨ paradigm.

Why Declarative Works?

Instead of instructing how to achieve a result, you precisely define what you want 🎯.

Advantages of the Declarative Paradigm:
Portable : Configurations can be easily shared and applied across environments.
Atomic : Operations are all✅-or-nothing❌, preventing partial application and inconsistent states. 
This eliminated the 🤕 headache of manually reverting changes after mid-script failures.
Reproducibility : Applying the same configuration should yield similar outcomes.
Tracked Changes : Differences from previous configurations are easily discernible at a glance.

This approach solves many app-level issues. But system-wide problems remain. 
The diverse toolchain, language specific package managers all doesn't follow the declarative paradigm. 
Even if they all follow, the problem isn't solved there. 
Because the environment on which they all work and depend on isn't the same. Which brings us to the next problem...

1.2) Dependency Hell 😈
❗Reason 1) TLDR: It's the culprit Imperative again
Even identical systems diverge over time as we install/update software/OS, run scripts etc etc. 
So 100 different system even if started and maintained the same way it won't be the same after some point. 
So over time we either lose track of changes or even if we track the change, the programs it depend on might vary and the scripts won't always work the same. Adjusting scripts for newer OS versions becomes routine 😮‍💨. That's just temporarily filling the cracks, not a perfect solution and also a headache to maintain and rollback if the script fails midway because of the nature of script(being imperative).

❗Reason 2) Existing nature of the conventional Package managers and repo models 📦
In most operating systems and languages, dependencies are managed globally. 
That means when you install a package, it often gets placed in a shared system path-like /usr/lib. 
And traditional package managers assume there's one "correct" state/version for the whole system. 

But in reality, every modern project might need a slightly different set of tools. 
Even if same tool, the versions might vary and even if same version, the flags might vary. 
Modern programs became super customized specific to their needs, 
so the traditional globally managed package managers doesn't allow 2 different versions of python(python 3.7 and 3.10.) to exist peacefully, 
needless to say about about customized flags of same versions. 
Even if you manage it, their own dependencies might conflict (e.g., libc versions 🧨).

Language-specific solutions like virtualenv help. 
But still the python versions coexistence problem exists and of course multiple languages having multiple such package managers only makes things complex to adopt the ecosystem and also making cross-language dependency management a nightmare 🧠💼. 
This brings the popular problem, "It works on my machine", but not working elsewhere.

Containers🐳:

The actual problem lies in the package management📦 level, but people did workarounds with VMs since it's a ready solution accepting the overhead😕. 
And people started to solve things from the VMs POV and made containers🐳. 
Container itself is a neat alternative for VMs giving more benefits than a VM. Yes, great. 
Now people who thought VM(virtualising hardware) as overhead started accepting the Container(virtualising kernel) as a better solution for scalability😞. 
Containers are used to solve the VMs but still an overhead for packaging and development environments😐. 
Especially to keep the environments in sync u have to manually intervene.

Don't get me wrong, containers are a great way for deployments👍 considering that's the only proper way for isolation other than VMs now. 
And the whole world is using docker and made their DevOps or any workflow that way. 
People went from VM route to Docker for solving scaling issues. But did not solve from the package management route🤧. 
This seems to solve the dependency hell and scalability, 
but still did not solve the imperative small cracks I mentioned at the start and bring another problem "Lack of Reproducibility". 
Especially the Dockerfile.


Why Dockerfile📜 is not good? (Guess the culprit again)

Let's have a very basic dockerfile example:

FROM ubuntu:latest
RUN apt-get update && \
 apt-get install -y curl wget && rm -rf /var/lib/apt/lists/*
WORKDIR /app
COPY . .
CMD ["curl" "--version"]

Dockerfile just again is another imperative thing which we just tell some instructions to arrive at a solution(Imperative). 
Now if u make 100 docker images from this file, all have the same environment right? 🤧Nope😳. 
If u create an OCI image with dockerfile today and after some months or years the 2 images vary because, 
again we use traditional package managers inside it , whose repo maintainers might :
drop certain version because of end of LTS  or
maybe they might have deleted wget or curl from their repo  and 
needless to say the change in their versions . 

One clever way is to make a package for every version commit in the source code . 
But all these traditional package managers ship the binaries they have already built. 
So it's not storage efficient to keep GBs or TBs of data for different version of a single package curl. 
And pinning the version in these commands is tedious and of course won't work the same after months or years if something in the repo changed. 
And all the problems we mentioned earlier still lives inside this container. 

Same Old Story, Different Surface:
Earlier: You made imperative steps(scripts) to set & patch things up in VMs.🙂
Now: You make imperative steps(dockerfile) to set & patch things up in Docker.🫠
Both creating the imperative cracks💔.
 
TLDR: Docker is nice💥, but dockerfile💀 isn't.

How do we actually solve 🤔: 
Imperative cracks? 
Diverging environments? ↔
Repetitive script failures? 
Inconsistent builds across systems? 

So how do we solve this decades of problem we face in every stage of SDLC, CICD, Deployments etc?
Is there any actual practical stable reliable ready solution available to tackle literally all these changes faced in IT?
Even if there is one, how many toolchain does it take to solve all these?
What would the world look like if we can actually reliably say "It works on my machine"?😳
Instead of accepting the overhead of maintaining the cracks etc, 
is there any such omnipotent like solution to actually solve all these?

The ultimate solution to all these is
Nix - The Revolutionary Declarative system for Determinism, Reliability and Precise control.
Behold the power of Nix and how it revolutionizes the world in the history of IT.


Nix is popularly synonymous to these:
(PF = Purely Functional -> Declarative -> Atomic -> Reliable)

Nix - The PF Programming Language
Nixpkgs - PF Package Manager
NixOS - PF Linux Distro
Specific Nix projects - PF [Shell, Containers, VMs, Custom ISOs etc] (Anything as Code)

Nix takes declarative a step ahead through purely functional approach.

Nix is a universal build bootstrapping tool, which brings a language agnostic way to build, cache and store artifacts.

Artifacts can be - Shells, packages, containers, VMs, Custom ISOs etc

Language Agnostic = Not tied to a single ecosystem like python, rust, etc

Nix Store : The Immutable Read Only Store
How it’s different from traditional PMs - Hashing, Symlinks. 
Hashing - Thus coexistence of any number of versions of same package🔥.
So every package builds from scratch?
That would take a long time to build right?
Caching - Reusing the artifacts 

Public cache - cache.nixos.org, etc

Local cache are stored in Nix store /nix/store along with other derivations.

Derivations = Artifact produced by nix, could be anything from text, file, OS etc.

Live graph: https://repology.org/repositories/graphs

Nixpkgs search: https://search.nixos.org/packages
(120,000 packages while preparing this PPT🔥)

Guess the size of the whole nixpkgs repo? 🤔

How many TBs or 100s of GBs?


Answer ->
It’s Just 5.05 GB🔥🔥🔥🔥🔥

Because everything are just bootstrapped nix files, all text , with all revisions of older packages too. It’s super efficient.

It caches from cache.nixos.org, but still u can build everything from scratch.

Enough with the theory, let’s have bird’s-eye view of Nix in action and see how we create a whole world bootstrapped from a single dependency i.e Nix.
Nix Shells: The virtual environment (Shells as Code)

#Enter a virtual environment shell with cowsay package
nix shell nixpkgs#cowsay

#U can run the app only once
echo "hello" | nix run nixpkgs#cowsay

