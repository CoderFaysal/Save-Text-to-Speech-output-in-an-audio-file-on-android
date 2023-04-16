# Save Text to Speech output in an audio file on android


```
private String mAudioFilename = "";
private final String mUtteranceID = "TextToSpeechAudio";
TextToSpeech textToSpeech;
private static final int REQUEST_PERMISSION_WRITE_EXTERNAL_STORAGE = 123;
 

btDone.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View v) {
        saveToAudioFile(editText.getText().toString().trim());
        CreateFile();
    }
});

   
   
   private void CreateFile() {
        // Perform the dynamic permission request
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M) {
            if (checkSelfPermission(Manifest.permission.WRITE_EXTERNAL_STORAGE) != PackageManager.PERMISSION_GRANTED)
                requestPermissions(new String[]{Manifest.permission.WRITE_EXTERNAL_STORAGE}, REQUEST_PERMISSION_WRITE_EXTERNAL_STORAGE);
        }
        // Create audio file location
        File sddir = new File(Environment.getExternalStoragePublicDirectory(Environment.DIRECTORY_DOWNLOADS), "Text To Speech Audio");
        if (!sddir.exists()) {
            boolean isDirectoryCreated = sddir.mkdirs();
            if (!isDirectoryCreated)
                Toast.makeText(this, "Can't create directory to save the Audio", Toast.LENGTH_SHORT).show();
        }
        sddir.mkdirs();

        mAudioFilename = sddir.getAbsolutePath() + "/" + mUtteranceID + System.currentTimeMillis() + ".wav";
    }

    private void saveToAudioFile(String text) {
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.LOLLIPOP) {
            textToSpeech.synthesizeToFile(text, null, new File(mAudioFilename), mUtteranceID);
            Toast.makeText(MainActivity.this, "Saved to " + mAudioFilename, Toast.LENGTH_LONG).show();
        } else {
            HashMap<String, String> hm = new HashMap();
            hm.put(TextToSpeech.Engine.KEY_PARAM_UTTERANCE_ID, mUtteranceID);
            textToSpeech.synthesizeToFile(text, hm, mAudioFilename);
            Toast.makeText(MainActivity.this, "Saved to " + mAudioFilename, Toast.LENGTH_LONG).show();
        }
    }
}
```
