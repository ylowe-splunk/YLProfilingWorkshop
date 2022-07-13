# Fixing The Code 

### 1. **Find the File**
<details>
  <summary>Talk Track</summary>
   To fix the bug, lets find to the DoorChecker file, and check out the precheck function.
</details>

Navigate to the DoorChecker file

```text
cd /Users/[NAME]/tracing-examples/profiling/workshop/src/main/java/com/splunk/profiling/workshop
```

Open the file
```text
vim DoorChecker.jar
```

---

### 2. **Investigate**

<details>
  <summary>Talk Track</summary>
   Scrolling down to the precheck function, We can see that the wait time increases exponentially to the index of the door, which means that door 3 is going to be much slower than door one. We dont want this feature, so we can just change this to `sleep(300)` 
</details>

Open the file and fix delay in the precheck function

=== "Original Code"
	```java
	private void precheck(int doorNum) {
        long extra = (int)Math.pow(70, doorNum);
        sleep(300 + extra);
    }
	```
=== "Corrected Code"
	```java
	private void precheck(int doorNum) {
        sleep(300);
    }
	```

---

### 3. **Rebuild and Rerun**

Navigate back to workshop directory
```text
cd /Users/[NAME]/tracing-examples/profiling/workshop
```

Rebuild the app
```text
./gradlew shadowJar
```

Rerun the app
```text
java -javaagent:splunk-otel-javaagent-all.jar \
    -Dsplunk.profiler.enabled=true \			
    -Dsplunk.profiler.call.stack.interval=1000 \
    -Dotel.resource.attributes=deployment.environment=workshop \
    -Dotel.service.name=profiling-workshop-[NAME] \
    -jar build/libs/profiling-workshop-all.jar
```

Select Door 3, and notice that the latency we previously saw is now gone. Now, we're done! Congrats!

---

### **Closing Remarks to SEs**

I find that the value proposition for this works best when it is simple and direct. We can help you find out which line of code is causing slowness in your application. You can use this as a jumping off point to talk about mttr and helping engineers save time, but at the end of the day, you don’t need a team of engineers to find a needle in a haystack. You just need a magnet. Positioning this as an elegant solution is what has made it work for me in the past, and I hope that is what will make it work for you in the future.













