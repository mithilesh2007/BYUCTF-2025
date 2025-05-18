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
- Then i found this string `bycnu)_aacGly~}tt+?=<_ML?f^i_vETkG+ b{nDJrVp6=)=` , where some storing is happening on it
- So i wrote a python code to print the sorted string

```python
str = "bycnu)_aacGly~}tt+?=<_ML?f^i_vETkG+ b{nDJrVp6=)="

for i in range(0, 23):
    index = (i * i) % 47
    print(str[index])

```

## Secure_Catalog

- firstly i used jad-x to view the source code then in the manifest file the main activity is the launcher activity
- in the main activity we need to find the credentials

```python
                    if (LoginActivity.this.emailID.getText().toString().equals(LoginActivity.this.getString(R.string.emailID)) && LoginActivity.this.password.getText().toString().equals(LoginActivity.this.getString(R.string.password))) {
```

- in this particular if block the user input(password and email) is being verified with a variable stored in `strings.xml` file (email= `admin@catalog.com`   , password = `password`)
- After entering into the app we can see some products (Device-1, Device-2, Device-3, Device-4)
- On clicking the Device 3 we can see a hint saying that “The product stores the secrets in tables ”
- Then i went to the apps internal storage were i found two encrypted databases (product-part 1.db , product-part 2.db)
- These databases are encrypted using AES
- In the manifest file there is also another activity called DBUtil

```jsx

    public List<String> getProducts(Activity activity, int i) {
        String str;
        ArrayList arrayList = new ArrayList();
        this.sqLiteOpenHelper = new DBUtil(activity, this.DBNAME);
        try {
            str = new String(Base64.decode("cGFzczEyMw==", 0), "UTF-8");
        } catch (Exception unused) {
            Log.d("Error", "Unable to access DB");
            str = null;
        }
        Cursor rawQuery = getReadableDatabase(str).rawQuery("SELECT * FROM machines", (String[]) null);
        if (rawQuery.moveToFirst()) {
            do {
                arrayList.add(rawQuery.getString(i));
            } while (rawQuery.moveToNext());
        }
        return arrayList;
    }
```

- In DBUtil class there is base64 encoded string which is when decrypt = pass123
- This key is used to decode the AES encryption
- Now while searching how to decrypt i came across some website links in string.xml file where i got to know that we can use `sqlcipher` to decrypt it

```bash
adb pull /data/data/smartcorp.catalog.secure/databases/product-part2.db
sqlcipher product-part2.db
PRAGMA key = 'pass123';
.tables
select * from secret;
cyberchaze{h4rdcoded_pa$s_y0u_g0t_th3_f1@g}
```

- then you will get the flag `cyberchaze{h4rdcoded_pa$s_y0u_g0t_th3_f1@g}`

## Specialist Beta

- firstly i used jad-x to view the source code then in the manifest file the main activity is the launcher activity
- After analyzing the main activity i found nothing
- So i analyzed Map Fragments activity
- In that activity i got to know that there is a if block only when the url is not null and above 14 and not equal to 27
- The host and the scheme is given in the manifest file and the path of the url is stored in the strings.xml file in a variable name `d1` (`ZGVlcGxpbmstc2VjcmV0LWludGVudC1kYXRh`)
- the decoded d1 is deeplink-secret-intent-data

```jsx
 if (data != null && String.valueOf(data).length() > 14 && String.valueOf(data).length() != 27) {
            String substring = String.valueOf(data).substring(14);
            try {
                str = new String(Base64.decode(getString(R.string.d1), 0), "UTF-8");
            } catch (UnsupportedEncodingException e) {
                e.printStackTrace();
                str = "";
            }
            if (substring.equals(str)) {
                textView.setText(Secrets());
                return;
            }
            return;
```

- commands used to get the flag

```jsx
adb shell am start -a android.intent.action.VIEW -d "webview://url/deeplink-secret-intent-data"
```

the flag is `cyberchaze{prot3ct_deeplink$_you_F0uNd_tHe_fl3g}`

## Super Secure Catalog

- firstly i used jad-x to view the source code then in the manifest file login activity is the launcher activity
- In the login activity there is a if block which is comparing our input and the data stored in the crypto class
- So i went to the crypto class where we can see some AES encryption going on . We can also see a part of a flag is hardcoded there  
`cyberchaze{th3_encRYpt3d_STR1ng_in_3nc6yPtE6_"`
- In this class we can see that there is some data being loaded from the binary file `in`
- Now decode the binary file using ghidra
- Then we can see some functions a ,b ,verify
- on analyzing the `A` function i got to know that there is an string converted to octal `154145156147164150137151163137066 164145145156041` . on decoding it we will get a sting “`length_is_6teen!`” which is the key for AES
- in the same way wile analyzing the b function there is another string converted to octal  `151040150141164145040163151170164145145156076074` . On decoding it we will get “`i hate sixteen><`” which is an initialization vector (IV)for AES
- There is another function called verify . ON analyzing it found a Hex encoded string `66621457d` so i decoded it using online tools . The decoded string is “`fi!E}` ”
- Then i thought of adding the string obtained from verify function and the hardcoded half flag in the crypto class which is “`cyberchaze{th3_encRYpt3d_STR1ng_in_3nc6yPtE6_fi !E}`”
