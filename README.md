# mininet-programming

Second project of Computer Network (CS391), written by java.
## How to run
The java files are managed by gradle. Use the following command
to build (if you don't have gradle, you can also 
use the bash script gradlew in the directory)
```
gradle build && gradle installDist
```
And then we can run the bash script `iperfer`
in the root directory as follows
```
./iperfer -s -p 2048
``` 
Then we run the python file. The result is in the 
`answer` directory.

## Dependency
mininet 2.2.2 (download from arch user repository by 
`yaourt -S mininet`)
