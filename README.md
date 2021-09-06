### Welcome to Timely Fitness

<img src="https://user-images.githubusercontent.com/58315985/132254364-3bcf827d-870c-4556-8c3a-37bfcc89484f.png" alt="drawing" width="200"/>

[![forthebadge](https://forthebadge.com/images/badges/built-for-android.svg)](https://forthebadge.com)
[![forthebadge](https://forthebadge.com/images/badges/made-with-java.svg)](https://forthebadge.com)
[![forthebadge](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://forthebadge.com)
[![forthebadge](https://forthebadge.com/images/badges/it-works-why.svg)](https://forthebadge.com)

Welcome to the Timely Fitness project!

Timely Fitness is an Android app that helps you keep good track of your fitness, and occasionally gives you
somewhat functional reminders to get moving.

### Want to help?

To create this project, I didn't just need to learn `Java`. I also had to use:

*   `XML`
*   `Markdown` (this file that you're reading right now is written in this scripting language,
    which offers functionality similar to that of `HTML`)
*   `Gradle` (a build tool that allows you to package runtime dependencies along with your
    compiled Java bytecode into something called a "build")


### How does the code behind the app work?

The app works through a central file called `MainActivity.java`.
It contains a crucial method in the code known as the "main" method, which the compiler looks for
when it tries to execute the app. Currently, the main function looks something like this:

    @Override
        public void onCreate(Bundle savedInstanceState) {

            SharedPreferences SP = PreferenceManager.getDefaultSharedPreferences(getBaseContext());
            String dayNightAuto = SP.getString("dayNightAuto", "2");
            int dayNightAutoValue;
            try {
                dayNightAutoValue = Integer.parseInt(dayNightAuto);
            } catch (NumberFormatException e) {
                dayNightAutoValue = 2;
            }
            if (dayNightAutoValue == getResources().getInteger(R.integer.dark_mode_value)) {
                AppCompatDelegate.setDefaultNightMode(AppCompatDelegate.MODE_NIGHT_YES);
                SweetAlertDialog.DARK_STYLE = true;
            } else if (dayNightAutoValue == getResources().getInteger(R.integer.light_mode_value)) {
                AppCompatDelegate.setDefaultNightMode(AppCompatDelegate.MODE_NIGHT_NO);
                SweetAlertDialog.DARK_STYLE = false;
            } else {
                AppCompatDelegate.setDefaultNightMode(AppCompatDelegate.MODE_NIGHT_FOLLOW_SYSTEM);
                int currentNightMode = getResources().getConfiguration().uiMode
                        & Configuration.UI_MODE_NIGHT_MASK;
                switch (currentNightMode) {
                    case Configuration.UI_MODE_NIGHT_YES:
                        SweetAlertDialog.DARK_STYLE = true;
                        break;
                    case Configuration.UI_MODE_NIGHT_NO:
                    default:
                        SweetAlertDialog.DARK_STYLE = false;
                }
            }

            super.onCreate(savedInstanceState);

            if (ContextCompat.checkSelfPermission(this,
                    Manifest.permission.WRITE_EXTERNAL_STORAGE)
                    == PackageManager.PERMISSION_GRANTED) {

                File folder = new File(Environment.getExternalStorageDirectory() + "/FastnFitness");
                boolean success = true;
                if (!folder.exists()) {
                    success = folder.mkdir();
                }
                if (success) {
                    folder = new File(Environment.getExternalStorageDirectory() + "/FastnFitness/crashreport");
                    success = folder.mkdir();
                }

                if (folder.exists()) {
                    if (!(Thread.getDefaultUncaughtExceptionHandler() instanceof CustomExceptionHandler)) {
                        Thread.setDefaultUncaughtExceptionHandler(new CustomExceptionHandler(
                                Environment.getExternalStorageDirectory() + "/FastnFitness/crashreport"));
                    }
                }
            }

            setContentView(R.layout.activity_main);

I'd agree with everyone's thoughts right now; it looks ***messy***. But the system has to check over
a lot of things, like the display size, the status of the device, and some strange abomination that
is the light mode/dark mode switcher thingy.

### Credits (to all the people whose code and library functions helped make this app just work)

Parts of the library functions for this project were forked from other awesome creations, such as
a project by [brodeurlv](https://www.github.com/brodeurlv) that is called
[FastNFitness](https://www.github/com/brodeurlv/fastnfitness). I found his `CSV` data parsing
functions extremely helpful, and included a folder of some of the functions that he wrote to parse
mine. I also used Google Code Snippets, which are folders of functions that the team behind the
Android mobile operating system wrote to help developers along, so I need to acknowledge that I
used their functional code to help me make this project possible.
## Timely Fitness
--------------------------
*    The official documentation and presentation for this Android application can be found at the GitHub Pages website for this repository:
     [https://anksharskarp.github.io/Timely-Fitness/](https://anksharskarp.github.io/Timely-Fitness/)
*    Make sure you read the [NOTICE.md](https://github.com/Anksharskarp/Timely-Fitness/blob/master/NOTICE.md) file for an important disclaimer
     about the external code/libraries/dependencies that I used in my project and how I gave credit to the code that others wrote.
*    I (William Zhang) am not responsible for any damages to devices if you download the APK from the releases tab. Up to this point, I have not tested the code,
     and cannot give any guarantees about the safety and quality of my code until I learn ~~to stop complaining about the incessant bugs in my code~~ to use JUnit or JBoss
     for testing.
*    If you want to contribute, email me at [aerialconnection98@gmail.com](aerialconnection98@gmail.com).
*    Credits to [brodeurlv](https://www.github.com/brodeurlv), because I used his Android app as a template for mine ~~to highlight how terrible I am at Android
     development~~(nobody saw that). The project's software license permitted the reuse of some of his `CSV` parsing functions, which were super helpful to me
     for data storage on one's local device. If you are interested in helping him out on his project, check it out at:
     *    [https://github.com/brodeurlv/fastnfitness](https://github.com/brodeurlv/fastnfitness)
--------------------------
