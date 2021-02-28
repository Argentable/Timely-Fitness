## Welcome to my Passion Project

[![forthebadge](https://forthebadge.com/images/badges/gluten-free.svg)](https://forthebadge.com)
[![forthebadge](https://forthebadge.com/images/badges/made-with-crayons.svg)](https://forthebadge.com)
[![forthebadge](https://forthebadge.com/images/badges/built-for-android.svg)](https://forthebadge.com)
[![forthebadge](https://forthebadge.com/images/badges/it-works-why.svg)](https://forthebadge.com)
[![forthebadge](https://forthebadge.com/images/badges/made-with-java.svg)](https://forthebadge.com)
[![forthebadge](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://forthebadge.com)

Welcome to my Passion Project (ignore the badge spam; you can add these to your own README from
[For the Badge](https://www.forthebadge.com)).

It's an Android app that helps you keep good track of your fitness, and occasionally gives you
somewhat functional reminders to get moving.

## Want to help?

To create this project, I didn't just need to learn `Java`. I also had to use:

*   `XML`
*   `Markdown` (this file that you're reading right now is written in this scripting language,
    which offers functionality similar to that of `HTML`)
*   `Gradle` (a build tool that allows you to package runtime dependencies along with your
    compiled Java bytecode into something called a "build")


## How does the code behind the app work?

20+ hours of me suffering through tutorials and the ~~wastebin fire that is Android Studio~~
(sorry, Android Studio and IntelliJ IDEA developers) :trollface:

Take that as a joke for the cringe work that I did.

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

So, how exactly do the files in the app work together as a framework?
Here is a handy text-based infographic to help.

    ### .github
    *   This is the folder that powers this website that you are looking at. It contains the YML files that determine the look and feel of this website, and also includes the index.md file, which decides the text and multimedia files that go along with the website.
    *   It also includes something called an ISSUE_TEMPLATE, which users can use to file complaints about the project, and give suggestions. The entire folder basically determines how my project and the project website is hosted on GitHub's servers
    *   Located on 2 branches: `main` and `gh-pages`
    
    ### .gradle
    *   Gradle is a useful build tool that helps me package the app into something called an APK archive. If you use Android, then you might be familiar with it. It contains all of the compiled .class files that my Java and XML code files compile to and autoinstalls itself on your phone. Gradle makes that possible. 
    *   This folder was automatically generated. I didn't write all of that :)
    
    ### .idea
    *   I use Android Studio, which is based off of the IntelliJ IDEA platform, a proprietory Integrated Development Environment for developing applications using Java or Kotlin. This folder continues settings for my IDE.
    *   This folder also was automatically generated.
    
    ### app
    *   The actual code for the app.
    
        #### java
        *   This contains the code that I completely wrote myself, including the backend logic of the application.
        *   This was the bulk of my project, and the *hardest* part. I had to learn about the Android API to make this.
        
        ### res
        *   I didn't understand or learn a lot of XML, so the visual part of the app is mostly auto-generated through the template that I used *and* the drag-and-drop interface in Android Studio. It looks like a lot, and I didn't write it; the system helped me.
    
    ### gradle
    *   More build tools for compiling my Java bytecode. 
    *   This folder was also automatically generated. 
    
    ### icons
    *   Icon and logo for the app. It only took some light Photoshop skills.
    
    ### other files
    *   .editorconfig is settings
    *   .gitignore is settings
    *   build.gradle is more settings for Gradle.
    *   gradle.properties is more settings for Gradle.
    *   gradlew is is more settings for Gradle.
    *   gradlew.bat is more settings for Gradle.
    *   LICENSE is the rules (which are no rules; my project is public domain).
    *   local.properties is settings for the exported user data.
    *   README.md is a short recap and intro about my project for users to see.
    *   settings.gradle is is more settings for Gradle (seemed kind of obvious from the start :p)

Hopefully, this helped you understand the code better.

## Credits (to all the people whose code and library functions helped make this app just work)

Parts of the library functions for this project were forked from other awesome creations, such as
a project by [brodeurlv](https://www.github.com/brodeurlv) that is called
[FastNFitness](https://www.github/com/brodeurlv/fastnfitness). I found his `CSV` data parsing
functions extremely helpful, and included a folder of some of the functions that he wrote to parse
mine. I also used Google Code Snippets, which are folders of functions that the team behind the
Android mobile operating system wrote to help developers along, so I need to acknowledge that I
used their functional code to help me make this project possible.

#### Contact GitHub Support if this website has issues

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.
