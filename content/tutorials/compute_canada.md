+++
title="Undergrad's guide to High Performance Computing"
date="2026-04-20"
[extra]
toc=false
[taxonomies]
tags=["Guides"]
+++

## Some context

At some point during research (if it's not experimental), you will require use of some sort of simulations.

Physics[^0] is, after all, a science of creating Good Enough<sup>TM</sup> models of the world, and those models have to be tested.
But even with the simplifications that we bring to the complex real world, when everything is a mass on a spring,
you simply trade the complexity of calculations for the amount.
And these amounts can get rather large. Large enough to require way more power than your laptop can provide.
Sometimes it remains possible, alas just slow. Sometimes, because of very high RAM requirements, it becomes simply not possible (I'm looking at you [meep](https://meep.readthedocs.io/en/latest/)).

In situations like these we have to turn to specialized equipment, made just for an occasion: supercomputers, and in our case, supercomputer clusters.

The concept of a HPC (High Performance Computing) cluster is rather simple:
If one computer can't do a task, we split this task into smaller chunks, and each of those chunks to different computers. They do the work, we combine the results,
and have our solution.

We will get more into details later, but that is the grand idea.

## Ok where exactly does it go, what does it mean, and who's Compute Canada?
 

Digital Research Alliance of Canada (previously known as Compute Canada) is the central national organization that operates[^1]
and manages resources for digital research infrastructure in Canada.

They manage resource allocations amongst members (researchers, such as your supervisor I assume), basically allocating CPU hours based on applications same way
as NSERC would assign grants based on research proposals[^2].

Depending on what type of work you do (CPU-bound computing, GPU-heavy bio simulations, or ML), different amount of space and CPU/GPU hours will be allocated
on different clusters, operated by the Alliance. There are a total of 8 clusters, 4 of them being [General Purpose](https://docs.alliancecan.ca/wiki/National_systems#Compute_clusters),
meaning designed for a diverse type of tasks, one designed for large [parallel tasks](https://docs.alliancecan.ca/wiki/Trillium)
where you use more then 1000 CPU cores at the same time, and 3 are GPU clusters 
designed for AI research.[^3]

And you, brave soul, will be given a key to these mighty machines, and command them to do your bidding.

## How?

So now starts the actual step by step instructions. Kind of. You will need to:

1. Get access code to register from your PI 
2. [Register on the website](https://docs.alliancecan.ca/wiki/Apply_for_a_CCDB_account), create a role, request access to the cluster your lab uses
3. [Set up 2FA](https://docs.alliancecan.ca/wiki/Multifactor_authentication) (please)(I think its mandatory now actually)
4. Open the terminal of your choosing, (or something like PuTTy on windows if you like suffering[^4])
5. [SSH](https://docs.alliancecan.ca/wiki/SSH) into the server you will be using.

Here, the paths diverge because what you have no is access to a Linux machine. If you want an intro there is one of course on the [wiki](https://docs.alliancecan.ca/wiki/Linux_introduction)
but the short and sweet is:

### Linux but real fast

```sh
[user@server ~]$ ls
```

Lists files in the current directory. Directory being the folder that you are in right now

```sh
[user@server ~]$ cd dir_name
```

Moves you into the directory named dir_name. Mind you all names are case sensitive so Dir_Name is not the same as dir_name.


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
you will never be lost on any server upon this earth because vim is shipped with EVERY Linux machine to ever exist.[^5]

In the bottom left, you will see in **bold** text **NORMAL**. That's the current *mode*. Vim has several modes, for doing different things, but the ones you need to know about are:


| Normal | The mode of entry, from which you go to every other mode. In here you just move around with arrows, and you can search by pressing `/` key, which you will see pop up below **NORMAL** text. From any other mode you return here by pressing `Esc` twice.|
|:--|:--|
| Input | Mode for text input. Here you are typing text as you would in any other text editor. Enter it by pressing `i` in **NORMAL** mode. |
| Command | Mode for issuing commands. Entered by pressing `/` or `:` in Normal mode. `/` is for searching, type whatever word you are looking for and the editor will jump there. `:` is for global commands, `:w` + `Enter` is to write changes to a file, `:wq` + `Enter` is to write and exit.|

Congats, you now know like 40% of how to use vim for simple things, go off and conquer.

{% end %}


If you like your sanity as it is now, use `nano`


```sh
[user@server ~]$ nano file_name
```

Very simple file editor, all the commands are at the bottom.

I will, however, REALLY recommend you edit and write all your files on your local computer, using whatever editor you want, and then transfer them to the server.

## Cool, now, what for?




\[WIP\] im still working on this







---
Sources:
1. [Research Alliance Canada wiki](https://docs.alliancecan.ca/wiki/Getting_started)

---
Remarks:
[^0]: I am making an example about physics because that's what I do.
[^1]: Technically, clusters are operated by separate entities and universities where they are located, but allocations are managed centrally by the Alliance.
[^2]: Ish. It's a bit* different the way comparisons are metaphors and not statements of equivalence but that's a problem you will have to worry about only when you are managing your own lab and therefore a bit out of scope of the guide.
[^3]: There are technically more, there is also a cloud specific instance, to run virtual machines, as well as [MonarQ](https://docs.alliancecan.ca/wiki/MonarQ/en), a 24-qubit
quantum computer operated by Calcul Québec. There are also 4 legacy clusters which are near their end of life and are being decommissioned but their storage is still online.
[^4]: Suffering is about using windows, not PuTTy per se.
[^5]: Some more simple ones might not have `vim`, but will have its predecessor, `vi`, which is literally the same and everything here works on both.
