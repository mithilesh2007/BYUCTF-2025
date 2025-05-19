# INTERNAL-CTFs


## BABY ANDROID 1

- firstly i used jad-x to view the source code then in the manifest file the main activity is the launcher activity
- In the main activity we can see that the utilities function is being cleaned up

```bash
public class MainActivity extends AppCompatActivity {
    @Override // androidx.fragment.app.FragmentActivity, androidx.activity.ComponentActivity, androidx.core.app.ComponentActivity, android.app.Activity
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Utilities util = new Utilities(this);
        util.cleanUp();
        TextView homeText = (TextView) findViewById(R.id.homeText);
        homeText.setText("Too slow!!");
    }
}
```

- So i navigated to the utilities class where there are some text views saying flag part 1,2,3,4, ……..
- Then i opened the layout xml file of the main activity where there is some text vies with some texts
- Now we have the xml code which has a flag but in a scrambled order
- So we can just copy it and past it any one of the application in android studio , so that we can see the sample view . which reveals the flag

## BABY ANDROID 2

- firstly i used jad-x to view the source code then in the manifest file the main activity is the launcher activity
- In the main activity there is an if block which checks weather our flag is correct or not with an internally stored flag
- So i double clicked on the `FlagChecker` which lead me to a class which is loading data from `babyandroid.lib`
- Now we need to decompile the binary file using ghidra
- Then i found this string `bycnu)_aacGly~}tt+?=<_ML?f^i_vETkG+b{nDJrVp6=)=` , where some storing is happening on it
- So i wrote a python code to print the sorted string

```python
str = "bycnu)_aacGly~}tt+?=<_ML?f^i_vETkG+ b{nDJrVp6=)="

for i in range(0, 23):
    index = (i * i) % 47
    print(str[index])

```

