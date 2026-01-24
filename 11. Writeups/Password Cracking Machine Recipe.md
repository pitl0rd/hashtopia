This guide is intended to aid in the construction of a password cracking machine. It is a guide, keep that in mind and don't let hardware limitations discourage you.  
-PeteTheHeat

##### System Architecture Scheme 

1 GPU best performs with 1 Dedicated CPU core/2 threads  
System memory should be designed: SystemMemory = GPU memory X 2  
Uniform RW speeds across storage devices is a plus when possible  
Use SSD for best performance

##### Scope 

Password cracking activities can be conducted on a wide range of systems. You can use an old mothballed system that might need a few parts to resurrect, a laptop, or you can construct a custom machine only governed by the theoretical limits of computational mathematics and physics. This material will try to cut a wide path as far as system design and cover the fundamentals when building out this type of device.

##### Hardware Considerations 

When you think of password cracking these days, you probably hear a lot about GPU. This is for good reason, GPU is superior when it comes to processing speeds on "fast" hashes. Now CPU is still very much a player in this space, and arguably is more widely used still to this day. From the outset a new system needs to have a clearly defined purpose:

```
- Will this machine ONLY be used for hash processing?
- Will this machine perform any analytical tasks?
- Will this machine operate in a configuration with other systems?
```

These are a few of the questions that you should ask right from the get go. The answers will determine the performance requirements of the system. For example, a machine that will only be used for hash processing is usually part of a system where analytics and storage are done on another computer(s). Considering the aforementioned system design, the password cracking machine would only really need to be built to maximize the efficiency of the machines GPU. Now if this system was going to perform analysis, you would want to make sure that sufficient CPU was available to do the processing work that you will require on that system. Same applies for storage, if you are planning on storing your wordlist archive on your password cracker, then you are going to want to design a large storage scheme with ample amounts of space available for working with wordlists.

##### Operating System Considerations 

I would highly recommend going with the operating system that you are most comfortable using. You are going to need to be comfortable using command line and working with large files. Linux is well suited for these tasks. I have used Windows platforms as well and since the implementation of WSL it is a much more capable platform.

##### Storage Considerations 

I recommend at least a 1 TB at a minimum, SSD preferred. You can go smaller and slower if you choose, but this is the minimum sweet spot for me. If you are hard up for storage or you are comfortable with more advanced material, you can use candidate generators to reduce the burden of HDD in your design.

##### Cost vs. Requirements 

Like most performance computer builds you are going to have to strategically hunt for deals to get the best bang for your buck.

_Part Priority_  
CPU  
GPU  
SSD  
RAM  
COOLING

Don't break the bank building the machine. As you read on you will see that the most effective password cracking tool that you have is your developed analytical ability to asses hashes that you are working with and the ability to utilize the product of that analysis to efficiently probe the target key-space.

#howto #notes #sudad