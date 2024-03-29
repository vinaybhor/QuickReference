Kotlin Dependencies:

// Room
    implementation("androidx.room:room-runtime:2.3.0")
    kapt("androidx.room:room-compiler:2.3.0")
    kapt("org.xerial:sqlite-jdbc:3.36.0.3")

    // Kotlin Extensions and Coroutines support for Room
    implementation("androidx.room:room-ktx:2.3.0")

    // Coroutines
    implementation("org.jetbrains.kotlinx:kotlinx-coroutines-core:1.5.2")
    implementation("org.jetbrains.kotlinx:kotlinx-coroutines-android:1.5.2-native-mt")
    implementation("org.jetbrains.kotlinx:kotlinx-coroutines-play-services:1.5.2-native-mt")

    // Coroutine Lifecycle Scopes
    implementation("androidx.lifecycle:lifecycle-viewmodel-ktx:2.4.0")
    implementation("androidx.lifecycle:lifecycle-runtime-ktx:2.4.0")







# QuickReference
======================================================
# Error Alert Dialog:

======================================================
# Iterate through list:

List<E> list;
for (E element : list) {
        element.getTitle();
    }
        
======================================================

# Get URI and file path from file chooser Intent
public static String getPath(final Uri uri, Context context) {
        final boolean isKitKat = Build.VERSION.SDK_INT >= Build.VERSION_CODES.KITKAT;
        if(isKitKat) {
            return getForApi19(context, uri);
        } else if ("content".equalsIgnoreCase(uri.getScheme())) {
            if (isGooglePhotosUri(uri)) {
                return uri.getLastPathSegment();
            }
            return getDataColumn(context, uri, null, null);
        }
        else if ("file".equalsIgnoreCase(uri.getScheme())) {
            return uri.getPath();
        }
        return null;
    }

    @TargetApi(19)
    private static String getForApi19(Context context, Uri uri) {
        Log.e(tag, "+++ API 19 URI :: " + uri);
        if (DocumentsContract.isDocumentUri(context, uri)) {
            Log.e(tag, "+++ Document URI");
            if (isExternalStorageDocument(uri)) {
                Log.e(tag, "+++ External Document URI");
                final String docId = DocumentsContract.getDocumentId(uri);
                final String[] split = docId.split(":");
                final String type = split[0];
                if ("primary".equalsIgnoreCase(type)) {
                    Log.e(tag, "+++ Primary External Document URI");
                    return Environment.getExternalStorageDirectory() + "/" + split[1];
                }
            }
            else if (isDownloadsDocument(uri)) {
                Log.e(tag, "+++ Downloads External Document URI");
                final String id = DocumentsContract.getDocumentId(uri);
                final Uri contentUri = ContentUris.withAppendedId(
                        Uri.parse("content://downloads/public_downloads"), Long.parseLong(id));

                return getDataColumn(context, contentUri, null, null);
            }
            else if (isMediaDocument(uri)) {
                Log.e(tag, "+++ Media Document URI");
                final String docId = DocumentsContract.getDocumentId(uri);
                final String[] split = docId.split(":");
                final String type = split[0];

                Uri contentUri = null;
                if ("image".equals(type)) {
                    Log.e(tag, "+++ Image Media Document URI");
                    contentUri = MediaStore.Images.Media.EXTERNAL_CONTENT_URI;

                } else if ("video".equals(type)) {
                    Log.e(tag, "+++ Video Media Document URI");
                    contentUri = MediaStore.Video.Media.EXTERNAL_CONTENT_URI;
                } else if ("audio".equals(type)) {
                    Log.e(tag, "+++ Audio Media Document URI");
                    contentUri = MediaStore.Audio.Media.EXTERNAL_CONTENT_URI;
                }

                final String selection = "_id=?";
                final String[] selectionArgs = new String[] {
                        split[1]
                };

                return getDataColumn(context, contentUri, selection, selectionArgs);
            }
        } else if ("content".equalsIgnoreCase(uri.getScheme())) {
            Log.e(tag, "+++ No DOCUMENT URI :: CONTENT ");
            if (isGooglePhotosUri(uri))
                return uri.getLastPathSegment();

            return getDataColumn(context, uri, null, null);
        }
        else if ("file".equalsIgnoreCase(uri.getScheme())) {
            Log.e(tag, "+++ No DOCUMENT URI :: FILE ");
            return uri.getPath();
        }
        return null;
    }

    private static String getDataColumn(Context context, Uri uri, String selection,
                                        String[] selectionArgs) {
        Cursor cursor = null;
        final String column = "_data";
        final String[] projection = {
                column
        };
        try {
            cursor = context.getContentResolver().query(uri, projection, selection, selectionArgs,
                    null);
            if (cursor != null && cursor.moveToFirst()) {
                final int index = cursor.getColumnIndexOrThrow(column);
                return cursor.getString(index);
            }
        } finally {
            if (cursor != null)
                cursor.close();
        }
        return null;
    }

    private static boolean isExternalStorageDocument(Uri uri) {
        return "com.android.externalstorage.documents".equals(uri.getAuthority());
    }

    private static boolean isDownloadsDocument(Uri uri) {
        return "com.android.providers.downloads.documents".equals(uri.getAuthority());
    }

    private static boolean isMediaDocument(Uri uri) {
        return "com.android.providers.media.documents".equals(uri.getAuthority());
    }

    private static boolean isGooglePhotosUri(Uri uri) {
        return "com.google.android.apps.photos.content".equals(uri.getAuthority());
    }
    
    
    #If doesn't work----below library
https://github.com/onimur/handle-path-oz
        
================================================================================================================================
        QR READER
implementation 'com.google.android.gms:play-services-vision:11.8.0'
--------
        Request Permission
--------
        private void requestPerm(){
    ActivityCompat.requestPermissions(ActivityBasicNavigation.this, new
            String[]{Manifest.permission.CAMERA, Manifest.permission.WRITE_EXTERNAL_STORAGE, Manifest.permission.RECORD_AUDIO}, REQUEST_MULTIPLE_PERMISSIONS);
}
--------
        Check Permission
--------
        private boolean checkPermission() {
        int cameraPerm = ActivityCompat.checkSelfPermission(ActivityBasicNavigation.this, Manifest.permission.CAMERA);
        int storagePerm = ActivityCompat.checkSelfPermission(ActivityBasicNavigation.this, Manifest.permission.WRITE_EXTERNAL_STORAGE);
        int audioPerm = ActivityCompat.checkSelfPermission(ActivityBasicNavigation.this, Manifest.permission.RECORD_AUDIO);

        return cameraPerm == PackageManager.PERMISSION_GRANTED &&
                storagePerm == PackageManager.PERMISSION_GRANTED &&
                audioPerm == PackageManager.PERMISSION_GRANTED;
    }
   -------
        
        if (checkPermission()){
                    Constants constants = new Constants();
                    Intent intent = new Intent(******.this, ********.class);
                    startActivityForResult(intent, 0);
                }else{
                    requestPerm();
                }
        
        -------------------------------------------------
        
        @Override
    public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
        super.onRequestPermissionsResult(requestCode, permissions, grantResults);
        if (requestCode == REQUEST_MULTIPLE_PERMISSIONS) {
            if (grantResults.length > 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED
                    && grantResults[1] == PackageManager.PERMISSION_GRANTED && grantResults[2] == PackageManager.PERMISSION_GRANTED) {
                // do Operation
                Intent intent = new Intent(****************.this, **********.class);
                startActivityForResult(intent, 0);
            } else {
                show_toastPermissions(ActivityBasicNavigation.this,"Permissions are Required to Proceed. Kindly Allow from settings");
            }
        }
    }
        
        -------------------------------------------------------------------
        
        <RelativeLayout android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="@dimen/dp14"
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:background="@color/colorblack">
    <FrameLayout
        android:layout_centerInParent="true"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="@drawable/rounded_corner"
        >
        <SurfaceView
            android:id="@+id/surfaceView"
            android:layout_width="match_parent"
            android:layout_height="350dp"
            android:layout_gravity="center_horizontal"
            android:layout_above="@+id/btnAction"
            android:background="@drawable/border_surfaceview"
            />
    </FrameLayout>


    <TextView
        android:id="@+id/txtBarcodeValue"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentTop="true"
        android:layout_marginLeft="@dimen/activity_horizontal_margin"
        android:layout_marginTop="@dimen/activity_horizontal_margin"
        android:layout_marginStart="@dimen/activity_horizontal_margin"
        android:text="Read QR code to start Authentication"
        android:textColor="@android:color/white"
        android:textSize="20sp" />


    <Button
        android:id="@+id/btnAction"
        android:backgroundTint="@color/colorblack"
        android:textColor="@color/white"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginBottom="@dimen/activity_horizontal_margin"
        android:layout_alignParentBottom="true"
        android:text="Close" />
</RelativeLayout>
        
        --------------------------------------------------------------------------
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="@dimen/activity_horizontal_margin">

    <SurfaceView
        android:id="@+id/surfaceView"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layout_above="@+id/btnAction"
        android:layout_alignParentLeft="true"
        android:layout_alignParentStart="true"
        android:layout_centerVertical="true" />

    <TextView
        android:id="@+id/txtBarcodeValue"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentTop="true"
        android:layout_marginLeft="@dimen/activity_horizontal_margin"
        android:layout_marginStart="@dimen/activity_horizontal_margin"
        android:text="No Barcode Detected"
        android:textColor="@android:color/white"
        android:textSize="20sp" />


    <Button
        android:id="@+id/btnAction"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_alignParentBottom="true"
        android:text="ADD CONTENT IN THE MAIL" />
</RelativeLayout>
        
        
 ===============================
        <androidx.drawerlayout.widget.DrawerLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/drawer_layout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:fitsSystemWindows="true"
    tools:openDrawer="start">

    <include
        layout="@layout/app_bar_basic_activity_navigation"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />

    <com.google.android.material.navigation.NavigationView
        android:id="@+id/nav_view"
        android:layout_width="wrap_content"
        android:layout_height="match_parent"
        android:layout_gravity="start"
        android:fitsSystemWindows="true"

         />

</androidx.drawerlayout.widget.DrawerLayout>
=======================================================================
        
        <style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
        <!-- Customize your theme here. -->
        <item name="colorPrimary">@color/colorPrimary</item>
        <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
        <item name="colorAccent">@color/colorAccent</item>
        <item name="windowActionBar">false</item>
        <item name="android:windowFullscreen">true</item>
        <item name="android:windowDisablePreview">true</item>
        <item name="android:windowAnimationStyle">@style/CustomActivityAnimation</item>
        <item name="windowNoTitle">true</item>
    </style>
========================================================================
        INTERNET Connection
=======================================================================
        public class InternetConnection {

    /** CHECK WHETHER INTERNET CONNECTION IS AVAILABLE OR NOT */
    public static boolean checkConnection(Context context) {
        final ConnectivityManager connMgr = (ConnectivityManager) context.getSystemService(Context.CONNECTIVITY_SERVICE);

        NetworkInfo activeNetworkInfo = connMgr.getActiveNetworkInfo();

        if (activeNetworkInfo != null) { // connected to the internet
            if (activeNetworkInfo.getType() == ConnectivityManager.TYPE_WIFI) {
                // connected to wifi
                return true;
            } else if (activeNetworkInfo.getType() == ConnectivityManager.TYPE_MOBILE) {
                // connected to the mobile provider's data plan
                return true;
            }
        }
        return false;
    }
}
============================================================================
        Digitally sign
        ---------------
        
        public String sign(String plainText, String alias) throws Exception {
        KeyStore keyStore=null;
        try {
            keyStore = KeyStore.getInstance("AndroidKeyStore");
            keyStore.load(null);

        }
        catch(Exception e) {}
        X509Certificate cert1 = (X509Certificate)keyStore.getCertificate(alias);
        KeyPair keyPair = new KeyPair(cert1.getPublicKey(), (PrivateKey) keyStore.getKey(alias, null));
        Signature privateSignature = Signature.getInstance("SHA256withRSA");
        privateSignature.initSign(keyPair.getPrivate());
        privateSignature.update(plainText.getBytes(UTF_8));

        byte[] signature = privateSignature.sign();

        return Base64.encodeToString(signature, Base64.DEFAULT);
    }
          ---------------
        verify digital sign
          ---------------
        public boolean verify(String plainText, String signature, String Publickey) throws Exception {
        KeyStore keyStore=null;
        try {
            keyStore = KeyStore.getInstance("AndroidKeyStore");
            keyStore.load(null);

        }
        catch(Exception e) {}
        CertificateFactory certificateFactory=CertificateFactory.getInstance("X.509");
        InputStream inputStream=new ByteArrayInputStream(Base64.decode(Publickey, Base64.DEFAULT));
        X509Certificate x509Certificate=(X509Certificate)certificateFactory.generateCertificate(inputStream);
        PublicKey publicKey =x509Certificate.getPublicKey();

        Signature publicSignature = Signature.getInstance("SHA256withRSA");
        publicSignature.initVerify(publicKey);
        publicSignature.update(plainText.getBytes(UTF_8));

        byte[] signatureBytes = Base64.decode(signature, Base64.DEFAULT);

        return publicSignature.verify(signatureBytes);
    }
        --------------
        Generate Keypair
        --------------
        public KeyPair show(Context context, String alias) {
//        try {
//            keyStore = KeyStore.getInstance("AndroidKeyStore");
//            keyStore.load(null);
//
//        }
//        catch(Exception e) {}

        try {
            // Create new key if needed
            //if (!keyStore.containsAlias(alias)) {
//                Toast toast = Toast.makeText(context, "Not", Toast.LENGTH_LONG);
//       toast.show();
            Calendar start = Calendar.getInstance();
            Calendar end = Calendar.getInstance();
            end.add(Calendar.YEAR, 1);
            KeyPairGeneratorSpec spec = new KeyPairGeneratorSpec.Builder(context)
                    .setAlias(alias)
                    .setSubject(new X500Principal("CN=RIL, O=RJIL"))
                    .setSerialNumber(BigInteger.ONE)
                    .setStartDate(start.getTime())
                    .setEndDate(end.getTime())
                    .build();
            KeyPairGenerator generator = KeyPairGenerator.getInstance("RSA", "AndroidKeyStore");
            generator.initialize(spec);

            keyPair = generator.generateKeyPair();

            // }
        } catch (Exception e) {
            Toast.makeText(context, "Exception " + e.getMessage() + " occured", Toast.LENGTH_LONG).show();
            // Log.e(TAG, Log.getStackTraceString(e));
        }
        return keyPair;
    }
        --------------
        Load Keys
        --------------
        KeyStore keyStore = KeyStore.getInstance("AndroidKeyStore");
keyStore.load(null);
KeyStore.Entry entry = keyStore.getEntry(alias, null);
PrivateKey privateKey = ((KeyStore.PrivateKeyEntry) entry).getPrivateKey();
PublicKey publicKey = keyStore.getCertificate(alias).getPublicKey();


------------------------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------------------------------------
<string name="optional_title">We have a new Update</string>
    <string name="optional_description">We have a new update waiting for you. Please update the app for best experience.</string>
    <string name="mandatory_title">Jio SecureID needs an update</string>
    <string name="mandatory_description">An update is now available. This is a mandatory update. Please update the app to continue using it. Thank you!</string>
