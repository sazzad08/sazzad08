

//Simple Prayer Time TimingByAddress
//mustbe INTERNET permission Allow





dependencies {

    implementation 'com.android.volley:volley:1.2.1'
    
}



public class MainActivity extends AppCompatActivity {

    
    TextView tv1,tv2,tv3,tv4,tv5,tvdate;

    DatePicker datePicker;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        
        tv1=findViewById(R.id.textView);
        tv2=findViewById(R.id.textView2);
        tv3=findViewById(R.id.textView3);
        tv4=findViewById(R.id.textView4);
        tv5=findViewById(R.id.textView5);
        tvdate=findViewById(R.id.textView6);
       

      



        RequestQueue requestQueue= Volley.newRequestQueue(this);

        //String currentdate link up this string url
        //address first city,country than method 1 is  "University of Islamic Sciences, Karachi"
        String url="https://api.aladhan.com/v1/timingsByAddress/+(currentdate)+?address=Lalmonirhat,BD&method=1";

        JsonObjectRequest jsonObjectRequest=new JsonObjectRequest(Request.Method.GET, url, null, new Response.Listener<JSONObject>() {
            @Override
            public void onResponse(JSONObject response) {

                try {
                    //Main JsonObject To Convert String
                    String code=response.getString("code");
                    String status=response.getString("status");

                    //There Is no code
                    //String data=response.getString("data");

                    if ( code.contains("200") && status.contains("OK")){

                        //second JsonObjectrequest to get jsonobject
                        JSONObject jsonObject=response.getJSONObject("data");

                        //second jsonObject to get thirdjsonObject
                        JSONObject timings=jsonObject.getJSONObject("timings");
                        JSONObject date=jsonObject.getJSONObject("date");


                        //jsonObject to convert String
                        String Fajr=timings.getString("Fajr");
                        String Dhuhr=timings.getString("Dhuhr");
                        String Asr=timings.getString("Asr");
                        String Maghrib=timings.getString("Maghrib");
                        String Isha=timings.getString("Isha");
                        String readable=date.getString("readable");


                        //textview settext this data
                        tv1.setText("Fajr :"+Fajr);
                        tv2.setText("Dhuhr :"+Dhuhr);
                        tv3.setText("Asr :"+Asr);
                        tv4.setText("Maghrib :"+Maghrib);
                        tv5.setText("Isha :"+Isha);
                        tvdate.setText(readable);



                    }

                } catch (JSONException e) {
                    throw new RuntimeException(e);
                }


            }
        }, new Response.ErrorListener() {
            @Override
            public void onErrorResponse(VolleyError error) {

            }
        });

        requestQueue.add(jsonObjectRequest);







    }
    //==========================
    //=============================
    String currentdate (){

        StringBuilder stringBuilder=new StringBuilder();
        stringBuilder.append(datePicker.getDayOfMonth()+"/");
        stringBuilder.append(datePicker.getMonth()+"/");
        stringBuilder.append(datePicker.getYear());

        return stringBuilder.toString();
    }

    //>>>>>>>>>>>>>>>>>>>>>>
}





