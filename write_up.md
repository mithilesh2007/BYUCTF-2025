# BABY ANDROID 1

- On installing the baby-android-1.apk , it displayed a message "Too slow!"
- Then i used jad-x to view the source code then in the manifest file the main activity is the launcher activity
- In the main activity we can see that the utilities function is being cleaned up by calling a clean up method
- ![image](https://github.com/user-attachments/assets/01cd5ecd-7a77-4eee-9088-2b06e170b8ce)
- So i navigated to the utilities class where there are some text view .
- ![image](https://github.com/user-attachments/assets/35925da7-a165-4ab3-8b7e-add486289cac)
- This cleanUp() method clears multiple TextView fields by setting their text to an empty string (setText("")). These TextView elements are named flagPart1, flagPart2, ..., flagPart28.
- Then i navigated to the xml file of the main activity inside Resource folder which contains 28 text views
- Each text view contains a letter which is in scrambled order
- ![image](https://github.com/user-attachments/assets/36421a69-969a-4185-8f79-6adcc621c55f)
- By looking at the layout marigin of each text view they have different vlaues which means they are loacted in some order .
- So to make things easier, I copied the XML layout into a dummy Android Studio project. This let me see the UI as it would appear on a real device, which helped me easily read the TextView content and piece together the full flag from the jumbled parts.
- ![image](https://github.com/user-attachments/assets/cd48f2d3-93ad-4742-87f9-6a234030ef07)

# BABY ANDROID 2
- On installing the baby_android-2.apk , it asked me to enter a flag for sanity check
  ![image](https://github.com/user-attachments/assets/85c32dcd-f2d9-4d2f-9185-9a2057766835)
- Then i used jad-x to view the source code , in the manifest file the i got to know that the main activity is the launcher activity
- In the main activity there is an if block which checks weather our flag is correct or not with an internally stored flag
- ![image](https://github.com/user-attachments/assets/7fb618f6-3e95-4998-b3d1-3e012947e72c)
- Here we can see that our input is being stored in `flagAttempt` varable and being sent to a method name `flagChecker`
- So i double clicked on the `flagChecker` which lead me to a class which is loading data from `babyandroid.lib`
- ![image](https://github.com/user-attachments/assets/3b57a73b-ae8a-46b6-8c19-e7ad275fcbbd)
- Then i decompile the binary file using ghidra
- ![image](https://github.com/user-attachments/assets/5079a6d2-82b9-4a7b-9a02-b9e7ce488e6a)
- On analyzing the c++ code , i understood that there a operation going on a string `bycnu)_aacGly~}tt+?=<_ML?f^i_vETkG+b{nDJrVp6=)=`
- So i wrote a python code to print the final string after the operation
```python
str = "bycnu)_aacGly~}tt+?=<_ML?f^i_vETkG+b{nDJrVp6=)="

output=""
for i in range(0, 23):
    index = (i * i) % 47
    output+=str[index]
print(output)

```
- The final output is byuctf{c++_in_an_apk??} , which passed the sanity check in the app
- ![image](https://github.com/user-attachments/assets/0c3079a7-681e-40cc-9c54-22931bcba99a)




