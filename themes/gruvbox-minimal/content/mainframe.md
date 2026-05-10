---
title: "Meditations on the mainframe"
draft: false
---
Since learning that mainframes are still very much [in use](https://www.youtube.com/watch?v=ouAG4vXFORc) in financial IT, where throughput and availability are paramount, I have taken an interest in the work on those machines. As few hobbyists have access to [their own mainframe](https://www.youtube.com/watch?v=wJyiHsfJLEI), such an interest also necessarily involves the work of emulating a markedly different machine architecture on your personal computer and getting an OS very much of the 70s running on it. In the modern world where there is a 5-step Medium.com tutorial for every new JS framework, this kind of slight esotericism is surprisingly delightful and left as an exercise to the reader. Rather, in this piece I will detail some of the lessons I've learned by working with an emulated version of MVS 3.8j as a young person mainly familiar with the Unix architecture. In other words, this should be read less as a tutorial of any kind and more so like the ramblings of a Dr. Frankenstein who has raised his monster and is now marveling at his idiosyncrasies through the medium of a blogpost.

## File structure
The first thing one might observe about MVS is the significantly different filesystem. It took me a bit of time getting used to the new terminology as well as some of the implications of the different structure. In the end it turns out most of the concepts have reasonably close analogues if you map the corresponding structures between the systems. Where stream-oriented files are the fundamental building block on Unix, MVS has record-oriented datasets. 

Datasets are organized in catalogues (directories) and use a hierarchical structure with levels separated by dots, such as "USER.DATA.REPORT". This is, however, just convention rather than a reflection of the actual underlying datastructure. The catalogues function more as lookup tables than anything else. From what I've been able to read, most of this structure is still true on IBM's modern-day Z/OS, not only because of technical momentum but because the approach offers certain advantages to the typical mainframe workloads, such as guaranteed atomic transactions by locking records rather than whole datasets or easy replacement of physical volumes since catalogues aren't tied to physical storage allocation.

However, to a new user this mostly manifests as a new set of lingo to learn, and the necessity of allocating format and block size of a dataset before use.

{{< figure src="/images/ALLOCATE.png" width="1000px" >}}

## Transaction processing
When you have familiarized yourself with the language of datasets composed of formatted records, aggregated into blocks, and physically allocated to cylinders or tracks, you can begin to write programs. A mainframe can run lots of different compilers, but as the batch- and transaction processing has been the thing drawing me to the mainframe, I have been mostly interested in JCL. 

On the face of it, JCL shouldn't be that difficult to learn given the remarkably small number of core statements. However, many of these accept dozens of different parameters, and the writing is further complicated by the column-specific syntax left over from the punch-card days.

However, if you manage to write a correct JCL program, it can then be submitted using a SUBMIT command, whereafter the (often deeply verbose) result can be read from the spool. As long as your jobs are relatively few or simple, this works fine. However, in enterprise setups handling high-volume transaction, CICS is used as middleware, enabling  stuff like interactivity during job sequencing and easier debugging. CICS is under its own license and is therefore not available out of the box, when emulating MVS, but yet again [hobbyists have come to the rescue](https://github.com/moshix/kicks). This is touching the edges of my current knowledge of this stuff, but it also beautifully illustrates why I find it interesting. 

> *The astonishment which I had at first experienced on this discovery soon gave place to delight and rapture. After so much time spent in painful labour, to arrive at once at the summit of my desires, was the most gratifying consummation of my toils.*
> 
> <cite>— Mary Shelley, Frankenstein</cite>





