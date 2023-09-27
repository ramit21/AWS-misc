# AWS-misc
Misc AWS learnings

Q. Process large no of messages from SQS. Order of messages doesn't matter.
A. USe EC2 instances in auto scaling group. Use multiple threads withn each EC2 instance. Read messages in batches.

Q. How to manage cold start times of Lambda?
A. 1. Use more memory => leads to more CPU=> faster processing on initial load.
2. USe interpreted languages like Nodejs, Python etc, instead of languages like .net and java which have heavy processing JIT compilation etc on initial load.
3. Use less dependencies in your lambda code, and keep the function lightweight.
4. Use provisoned concurrency feature of Lambda.
5. a btach job to invoke Lambda at regular intervals with a dummy payload to keep it warm.

Q. Mobile app that supports offline sync when internet comes back.
A. Amplify Data store helps sync data on your device with AWs storage.



