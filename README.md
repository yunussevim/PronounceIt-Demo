# Pronounce It

## Introduction
Pronounce It is a game based on Hms Automatic Speech Recognition and Text to Speech features. It is aimed to use these technologies widely in practice and to facilitate their use by developers. We developed this game for users who want to measure and improve their pronunciation while speaking English. In this game users pronounce the sentences displayed on the screen and the accuracy of this pronunciation was measured. This accuracy value shows the user's success in the game. Pronounce It is playable in different categories, keeps leaderboard scores, supports two language and most importantly the whole app can be voice controlled to use ASR more frequently. 

For details about the service introduction and access guide, visit the following website: [HUAWEI ML Kit Development Guide] (https://developer.huawei.com/consumer/en/doc/development/HMSCore-Guides-V5/service-introduction-0000001050040017-V5) [HUAWEI ML Kit API Reference] (https://developer.huawei.com/consumer/en/doc/development/HMSCore-References-V5/asrsdkoverview-0000001050747393-V5)

The application has a voice command feature. The whole application can be managed with voice commands. This feature has been integrated to use ASR technology more widely.

## Voice Command
HMS ASR is used both in the game screen of the application and in other screens with the voice command feature. Therefore, a helper class 'SpeechRecognition' that encapsulates the implementation is created to simplify its use. `SpeechRecognition` class. ASR has two modes. In one mode `ALLINONE`, the sentence is written on the screen after the speech is finished, while in the other mode `WORDFLUX`, word by word can be written on the screen while speaking. This mode can be change with 'changeListeninFeature' method:

```
private void changeListeningFeature(int feature) {
        mSpeechRecognizerIntent
                .putExtra(MLAsrConstants.FEATURE, feature);
    }
```

Huawei supports Speech Pickup UI to record the voiced input that looks like bottom sheet dialog. But this UI is not used in this app. Custom UI layout `custom_asr.xml` is created for voice command feature instead of HMS Pickup UI. Voice Command feature can be activated in preferences page. During voice recognition, a sheet covers the screen and the recognized voice is written over that sheet. It is intended for the user to understand that they should manage the application by voice, not by touch. If the voice command is active, it starts listening to the user when the application page is opened. With the touch of user over the sheet Voice Command feature is disabled for that page. 


(Ekran görüntüleri)



Manager class `VoiceCommandManager` is created to use voice command feature on all pages. In this class `startVoiceCommandListening` method is called from application pages. In this method, the custom layout `custom_asr.xml` is brought to the front of the page and then starts to listen user. During the listening writes the speech of user into chosen textview. After listening is complete custom layout becomes invisible an in unreachable from user. `startVoiceCommandListening` method listens user and classifies user's purpose. Classification was implemented simply without using a large dataset or machine language algorithms. In manager class `classifiedInput` method takes speech and finds which task is intended. It is enough to have defined keywords in the speech for voice command to understand the task. User can say "Go preferences" or "Open preferences" but "preferences" is enough to understand the task.

After voice recognition is finished, Activities should manage the application according to the user speech. `VoiceCommandManager` class has an interface `ASRListener`. The activities implement this interface and call the method `onSpeechComplete` within it. This method is called when speech recognition is finished. Activities acts on output of 'classifiedInput' method. You can see examples below:

### Use Cases
        
#### Home Page Usages
..* Play        | Navigates to play screen.
..* Leader      | Navigates to leader board.
..* Preferences | Navigates to prefrences screen.


