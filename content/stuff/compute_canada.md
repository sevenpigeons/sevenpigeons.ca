+++
title="Undergrad's guide to High Performance Computing"
date="2026-04-20"
[extra]
toc=false
mermaid=true
[taxonomies]
tags=["Guides"]
+++

## Some context

At some point during research (if it's not experimental), chances are, you will require use of some sort of simulations.

Physics[^1] is, after all, a science of creating Good Enough<sup>TM</sup> models of the world, and those models have to be tested.
But even with the simplifications that we make, when everything is a mass on a spring,
you simply trade the complexity of calculations for their amount.
And these amounts can get astronomically large. Large enough to require more power than any local resources you might have access to can provide.
Sometimes it remains feasible, alas just slow. Sometimes, because of stupendous RAM requirements, it becomes simply not possible (I'm looking at you [meep](https://meep.readthedocs.io/en/latest/)).

In situations like these we have to turn to specialized equipment, made just for such occasion: supercomputers, and in our case, supercomputer clusters.

The concept of a HPC (High Performance Computing) cluster is rather simple:
If one computer can't do a task, we split this task into smaller chunks, and send each of those chunks to different computers. They do the work, we combine the results,
and have our solution.

We will get more into details later, but that is the grand idea.

## Ok where exactly does it go, what does it mean, and who's Compute Canada?
 

Digital Research Alliance of Canada (previously known as Compute Canada) is the central national organization that operates[^2]
and manages resources for digital research infrastructure in Canada.

They manage resource allocations amongst members (researchers, such as your supervisor I assume), basically allocating CPU hours based on applications same way
as NSERC would assign grants based on research proposals[^3].

Depending on what type of work you do (CPU-bound computing, GPU-heavy bio simulations, or ML), different amount of space and CPU/GPU hours will be allocated
on different clusters, operated by the Alliance. There are a total of 8 clusters, 4 of them being [General Purpose](https://docs.alliancecan.ca/wiki/National_systems#Compute_clusters),
meaning designed for a diverse type of tasks, one designed for large [parallel tasks](https://docs.alliancecan.ca/wiki/Trillium)
where you use more then 1000 CPU cores at the same time, and 3 are GPU clusters 
designed for AI research.[^4]

And you, brave soul, will be given a key to these mighty machines, and command them to do your bidding.

## How?

So now starts the actual step by step instructions. Kind of. You will need to:

1. Get access code to register from your PI 
2. [Register on the website](https://docs.alliancecan.ca/wiki/Apply_for_a_CCDB_account), create a role, request access to the cluster your lab uses
3. [Set up 2FA](https://docs.alliancecan.ca/wiki/Multifactor_authentication) (please)(I think its mandatory now actually)
4. Open the terminal of your choosing, (or something like PuTTy on windows if you like suffering[^5])
5. [SSH](https://docs.alliancecan.ca/wiki/SSH) into the server you will be using.

Here, the paths diverge because what you have now is access to a Linux machine. If you want an intro there is one of course on the [wiki](https://docs.alliancecan.ca/wiki/Linux_introduction)
but the short and sweet is:

### Linux but real fast

Everything in Linux is a file. What you are currently "in" is your home folder. 
The location is specifically `/home/$your_username/` (`$` is used for variables, so here replace it with your username, whatever it is.)

If you want to double check it, you can run the command:

```sh
[user@server ~]$ pwd
```

To *p*rint *w*orking *d*irectory.


```sh
[user@server ~]$ ls
```

Lists files in the current directory. Directory being the folder that you are in right now

```sh
[user@server ~]$ cd dir_name
```

Moves you into the directory named `dir_name`. Mind you all names are case sensitive so `Dir_Name` is not the same as `dir_name`.


```sh
[user@server ~]$ less file_name

[user@server ~]$ cat file_name
```

Will display the file. `less` will show it in a program, that you can search and move up and down, `cat` will output the contents of the file directly in the terminal window that
that you are in.

To edit the file, you have a choice:

If you are brave, open and read about VIM ([mandatory xkcd reference](https://xkcd.com/378/))

{% collapse(title="### VIM") %}

```sh
[user@server ~]$ vim file_name
```

Ok ok this is scary you are using vim yes everything you heard about it is true but BUT listen to me. LISTEN it's simple I promise, and useful because once you learn this,
you will never be lost on any server upon this earth because vim is shipped with EVERY Linux machine to ever exist.[^6]

In the bottom left, you will see in **bold** text **NORMAL**. That's the current *mode*. Vim has several modes, for doing different things, but the ones you need to know about are:


| Normal | The mode of entry, from which you go to every other mode. In here you just move around with arrows, and you can search by pressing `/` key, which you will see pop up below **NORMAL** text. From any other mode you return here by pressing `Esc` twice.|
|:--|:--|
| Input | Mode for text input. Here you are typing text as you would in any other text editor. Enter it by pressing `i` in **NORMAL** mode. |
| Command | Mode for issuing commands. Entered by pressing `/` or `:` in Normal mode. `/` is for searching, type whatever word you are looking for and the editor will jump there. `:` is for global commands, `:w` + `Enter` is to write changes to a file, `:wq` + `Enter` is to write and exit.|

Congrats, you now know like 40% of how to use vim for simple things, go off and conquer.

{% end %}


If you like your sanity as it is now, use `nano`


```sh
[user@server ~]$ nano file_name
```

Very simple file editor, all the commands are listed at the bottom.

I will, however, REALLY recommend you edit and write all your files on your local computer, using whatever editor you want, and then transfer them to the server.

Either using the web interface of [Globus](https://docs.alliancecan.ca/wiki/Globus) or something in the terminal like `scp` or `rsync`.

Do check the appropriate [wiki page](https://docs.alliancecan.ca/wiki/Transferring_data) about transferring data for more info.

## SLURM and operating clusters.

All of this was preface to what to do once you are SSH'd into the cluster.

Once you have logged-in via the terminal (SSH'd) into the server, you are (as mentioned before) in a Linux system.

More specifically, you are inside whats called a **Login Node**.

Login node of a system running SLURM, a Simple Linux Utility for Resource Management. Piece of software designed to organize and well, manage resources in Linuc clusters.

Let's show a little diagram.


{% mermaid() %}

---
title: SLURM cluster structure
config:
  theme: dark
---

graph TD
A[Your computer] -->|SSH| B(Login Node) 
subgraph cluster
B <--> C{Compute Node}
B <--> D{Compute Node}
B <--> E{Compute Node}
B <--> F{Compute Node}
C --> G[(Shared Resources)] 
D --> G
E --> G 
F --> G
end
{% end %}

So. As you have seen on the wiki, every cluster is composed of a certain amount of Nodes. 
For security and general IT well-being reasons, often those nodes are isolated from the world, and can only talk to each other.

So to talk to them there are special nodes, called Login Nodes, which are way less powerful then a standard node,
and exist to submit jobs to the rest of the cluster. Alliance IT people will get rather mad if you are doing something computationally intensive 
on them because it will slow down work for quite literally _<b>EVERYONE ELSE</b>_ using the cluster and you might receive a strongly worded email.

Can't recommend.

Everything longer then 3 minutes you will have to submit to the cluster using `sbatch`.

`sbatch` as input takes what's called a job file: a bash script that describes the job you want to run.
It's composed out of configuration part, where you tell SLURM what resources you would like to use, and the actual commands to run the program you want.

Please please [please please at least glance on the appropriate wiki page](https://docs.alliancecan.ca/wiki/Running_jobs).
It will do a much better job than I can.

But the basic form is: 

```sh 
#!/bin/bash
#SBATCH --account=def-your_supervisor_thingy_here
#SBATCH --time=48:00:00    //total time you request for your job

module load StdEnv/2023

./my_program --input file.csv

```

If it seems intimidating, it's okey, you'll get used to it and comfortable adjusting *really* quickly.

To explain step by step,

`--account` is the account from which your allocation will be taken. It's the project identifier of your supervisor, under which they received resources for the cluster.
Ask your supervisor which one to use, you shouldn't need to change it that often.

`--time` is the maximum runtime that your job has to run. After it runs if your job is still running it WILL get cancelled so either 
1. Make sure it has enough time
2. Make sure you can restart it from some checkpoint to continue the progress

`module load ____` is what you use to make different programs accessible to your code.
By default you only have Standard Environment, so basic linux commands, and if you want anything else (like, lets say python for example)
you would need to load it, using
`module load python/3.13`.

Look up the wiki to see what [Available software](https://docs.alliancecan.ca/wiki/Available_software) they have and how to properly load it.

There is a whole array of different other parameters you can specify, like:

`--cpus-per-task=4` to tell how many CPUs you want for every job launched.

`--mem=16G` to specify how much RAM per node you want.

`--job-name=cool_job_name` to rename your job from the default schema to something specific, to better keep track of them.

`--output=logs/log_good.out` To specify where the output goes. By default, all program output (both just logs and errors) are placed inside files called `slurm-$id` where $id is the
ID of the job. It can get pretty annoying and confusing when you have 3000 of these log files littering about, so you can specify where exactly and how to name the log files.

`--error=logs/log_err.err` if you want to make your control even more granular, you can specify where to place only errors from your program running. All normal logs will go to the 
`--output` location, and errors into this error file.

`./my_program --input file.csv` is the rest of the job, the part describing what programs you actually want to run and with what files.
This part is fully up to you, and depends on *what you are actually doing*, so here I am of little help.
But! the wiki might not be! Look up the software you are using and you might find recommended settings and methods of running, as well as common problems you might face.

General rule of thumb, is that the more resources you ask for, the longer you will wait in queue.
A job that needs 1 core for 4 hours is much much easier to fit somewhere then a beheamoth of a request for 4 whole nodes, all CPUs and RAM inside included.
Don't cheap out on the resources you request, do leave yourself some headroom; "I know this job needs 12 GB of RAM so I will give 12.1 GB" is not a great idea.
But don't ask for 3X the amount *just because*, otherwise list of deadlines you will be worrying about might also now include the heat death of the universe.

If you want any further reading, or just more details, I recommend nothing else but the [man pages](https://slurm.schedmd.com/sbatch.html#SECTION_OPTIONS) for the `sbatch` command 
on the official slurm website. They have stellar documentation, albeit a bit verbose. I'd recommend sticking with Alliances wiki for starters.

Once you have submitted your job, second command you need to know about is `sq`. It will queue the list of all running jobs and show you what jobs you submitted, and in 
what state they are right now, either PD(Pending) or R(Running), as well as other useful information like how much time is left what nodes it's running on and so on.

To quote Alliance wiki, `Do not run sq or squeue from a script or program at high frequency`. Every single time it runs a request to slurm's database and if everyone is
constantly asking "Is my job done yet???" it can get compounded to some serious system load. :(  That would be bad.


Once your job finishes, well, congrats! You are done[^7], and can transfer the results back and do whatever analysis you need.

So..... yeah?


---
Sources:
1. [Research Alliance Canada wiki](https://docs.alliancecan.ca/wiki/Getting_started)
2. [SLURM Documentation](https://slurm.schedmd.com/)
3. Voices from the dark

---
Remarks:
[^1]: I am making an example about physics because that's what I do.
[^2]: Technically, clusters are operated by separate entities and universities where they are located, but allocations are managed centrally by the Alliance.
[^3]: Ish. It's a bit* different the way comparisons are metaphors and not statements of equivalence but that's a problem you will have to worry about only when you are managing your own lab and therefore a bit out of scope of the guide.
[^4]: There are technically more, there is also a cloud specific instance, to run virtual machines, as well as [MonarQ](https://docs.alliancecan.ca/wiki/MonarQ/en), a 24-qubit
quantum computer operated by Calcul Québec. There are also 4 legacy clusters which are near their end of life and are being decommissioned but their storage is still online.
[^5]: Suffering is about using windows, not PuTTy per se.
[^6]: Some more simple ones might not have `vim`, but will have its predecessor, `vi`, which is literally the same and everything here works on both.
[^7]: None of this is truly ever done and you will run into so many more issues during your quest, but thats details. If anything does go wrong do email `support@tech.alliancecan.ca`.
AllianceCan employs some actual wizards and they have most probably already seen it all, and have a solution just for you read and wrapped somewhere.
