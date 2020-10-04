# android-call-api-using-volley

## app/build.gradle

com.android.volley:volley:1.1.1

## AndroidManifest.xml

uses-permission android:name="android.permission.INTERNET"

## MainActivity.java

```java
public void onGetPlayersClick(View view) {
    final TextView textView = (TextView)findViewById(R.id.textView);

    Response.Listener<JSONArray> successHandler = new Response.Listener<JSONArray>() {
        @Override
        public void onResponse(JSONArray response) {
            StringBuilder builder = new StringBuilder();
            for (int i = 0; i < response.length(); i++) {
                try {
                    JSONObject player = response.getJSONObject(i);
                    String firstName = player.getString("firstName");
                    builder.append(firstName);
                    builder.append("; ");
                } catch (JSONException e) {
                    builder.append(e.getMessage());
                }
            }
            textView.setText(builder);
        }
    };

    Response.ErrorListener errorHandler = new Response.ErrorListener() {
        @Override
        public void onErrorResponse(VolleyError error) {
            textView.setText("That didn't work");
        }
    };

    RequestQueue queue = Volley.newRequestQueue(this);
    JsonArrayRequest jsonArrayRequest = new JsonArrayRequest(
            Request.Method.GET,
            "http://fake-name-12345.herokuapp.com/players",
            null,
            successHandler,
            errorHandler
    );
    queue.add(jsonArrayRequest);
}
```

## activity_main.xml

```xml
<?xml version="1.0" encoding="utf-8" ?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
              xmlns:tools="http://schemas.android.com/tools"
              android:layout_width="match_parent"
              android:layout_height="match_parent"
              android:orientation="vertical"
              tools:context=".MainActivity">

      <Button android:layout_width="match_parent"
              android:layout_height="wrap_content"
              android:text="GET PLAYERS"
              android:onClick="onGetPlayersClick" />

      <TextView android:id="@+id/textView"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="Hello World!" />

</LinearLayout>
```
