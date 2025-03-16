# Friends of Friends using MapReduce

## Overview
The "Friends of Friends" project is a Hadoop-based MapReduce program that analyzes a social network dataset to find mutual friends and generate friend recommendations based on common connections.

## Components
The project consists of the following Java files:

1. **FriendsOfFriendsDriver.java** - The main driver program that orchestrates the execution of two MapReduce jobs.
2. **MutualFriendsMapper.java** - A mapper that generates pairs of users and their friend lists.
3. **MutualFriendsReducer.java** - A reducer that finds mutual friends between user pairs.
4. **FriendsRecommendationMapper.java** - A mapper that generates friend recommendations based on mutual connections.
5. **FriendsRecommendationReducer.java** - A reducer that ranks recommendations based on the number of mutual connections.
6. **social_network.txt** - The input dataset representing user friendships.

## Workflow
The program runs in two MapReduce jobs:

### Job 1: Finding Mutual Friends
1. **Mapper (MutualFriendsMapper.java)**
   - Reads the input dataset where each line represents a user and their direct friends.
   - Emits key-value pairs where the key is a user pair and the value is the friend's list.

2. **Reducer (MutualFriendsReducer.java)**
   - Receives user pairs and their friend lists.
   - Finds the intersection (mutual friends) between the two lists.
   - Emits user pairs along with their mutual friends.

### Job 2: Generating Friend Recommendations
1. **Mapper (FriendsRecommendationMapper.java)**
   - Reads the mutual friends data from Job 1.
   - Emits key-value pairs where the key is a user and the value is a mutual friend.

2. **Reducer (FriendsRecommendationReducer.java)**
   - Counts the occurrences of each recommended friend.
   - Sorts the recommendations in descending order of mutual friend count.
   - Emits the final friend recommendations for each user.

## Input Format
The input file (`social_network.txt`) follows this format:
```
User<TAB>Friend1,Friend2,Friend3
```
Example:
```
A    B,C,D
B    A,D,E
C    A,D,E
D    A,B,C,E
E    B,C,D
```

## Output Format
The output will be a list of friend recommendations for each user, ranked by the number of mutual friends.
Example:
```
A    E(2)
B    C(1)
C    B(1)
D    -
E    A(2)
```

## How to Run
1. Compile the Java files into a JAR:
   ```
   hadoop com.sun.tools.javac.Main *.java
   jar cf fof.jar *.class
   ```
2. Run the program on Hadoop:
   ```
   hadoop jar fof.jar FriendsOfFriendsDriver /input/social_network.txt /output1 /output2
   ```
3. Retrieve the results:
   ```
   hdfs dfs -cat /output2/part-r-00000
   ```

## Dependencies
- Hadoop 3.x
- Java 8+

## Conclusion
This project demonstrates how to use MapReduce to analyze social network data, find mutual friends, and generate friend recommendations efficiently using distributed computing.
