---
layout: post
header-img: img/default-blog-pic.jpg
author: pranjan
description: 
post_id: 4435
created: 2010/08/16 00:29:39
created_gmt: 2010/08/15 19:29:39
comment_status: open
---

# Database implementation in Android

<p style="text-align: justify;">Android uses SQLLite, which is an open source SQL database system for storing persistent data. The database that you create for an application is only accessible to itself, others applications won’t be able to access it. Android provides API’s to interact with the database which are quite easy to use. The performance of the database also seems to be good.</p>

<p style="text-align: justify;">In this post we will be creating the DatabaseUtility classes which will encapsulate the database centric functionality. Then we will use the utility to create more specific DAO classes which will encapsulate more fine grained functions of talking to the database.</p>

<p style="text-align: justify;">Let's start with the creation of the database utility. First of all we need to extend the SQLiteOpenHelper class.</p>

<blockquote>[sourcecode language="java"]
public class DatabaseHelper extends SQLiteOpenHelper {

 static final String DATABASE_NAME = &quot;records&quot;;
 static final int DATABASE_VERSION = 1;

 static final String TABLE_CREATE = &quot;create table address (id integer primary key autoincrement, &quot; + &quot;city text not null, street text not null,     &quot; + &quot;country text not null);&quot;;

 DatabaseHelper(Context context) {
      super(context, DATABASE_NAME, null, DATABASE_VERSION);
 }

 @Override
 public void onCreate(SQLiteDatabase db) {
      db.execSQL(TABLE_CREATE);
 }

 @Override
 public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
      db.execSQL(&quot;DROP TABLE IF EXISTS address&quot;);
      onCreate(db);
 }
}
[/sourcecode]

<!--more--></blockquote>

<p style="text-align: justify;">It requires a constructor taking a context. The context in our case will be the Activity from which the DatabaseHelper will be created.  The super constructor is called with the database name and database version. If the database asked does not exists, then a new database is created. The onCreate() and onUpgrade() methods are also needed. The onCreate() method is a callback method used to create the database and the other callback method onUpgrade() is used to delete the old table and create new if a new version of database is specified.</p>

<p style="text-align: justify;">The onCreate() here simply executes a sql query to create the table. The onUpgrade() drops the already existing table and creates a new one.</p>

<p style="text-align: justify;">Now we have a database and table created. We will proceed to insert data into the table. For this we will create an AddressDAO class which will operate on the address table created using the DatabaseHelper class.</p>

<blockquote>[sourcecode language="java"]

public class AddressDAO {

static final String TABLE_NAME = &quot;address&quot;;

static final String ID = &quot;id&quot;;
static final String CITY = &quot;city&quot;;
static final String STREET = &quot;street&quot;;
static final String COUNTRY = &quot;country&quot;;

private final Context androidContext;

private DatabaseHelper databaseHelper;
private SQLiteDatabase db;

public AddressDAO(Context ctx) {
     this.androidContext = ctx;
     databaseHelper = new DatabaseHelper(androidContext);
}

...
}

[/sourcecode]</blockquote>

<p style="text-align: justify;">Here we have created variables like table name, column names , the context and a constructor used to instantiate the DatabaseHelper. The DatabaseHelper’s instantiation ensures the presense of an address table.</p>

<p style="text-align: justify;">Now we can insert utility methods in the AddressDAO. Let’s start with opening the databse. The open() simple gets a writeble database using the DatabaseHelper’s superclass’s methods. The close simply closes the writeble database.</p>

<blockquote>[sourcecode language="java"]

public void open() throws SQLException {
     db = databaseHelper.getWritableDatabase();
}

[/sourcecode]

[sourcecode language="java"]

public void close() {
     databaseHelper.close();
}

[/sourcecode]</blockquote>

<p style="text-align: justify;">The insert method will insert city,street, and country in their respective columns. It uses ContentValues classes which stores data as key value pairs. The key being the column name and the value being the data to be stored in that column. It uses databaseHelper’s insert method, which asks for the table name and the values to be inserted.</p>

<blockquote>[sourcecode language="java"]

public long insert(String city, String street, String country) {
     ContentValues values = new ContentValues();
     values.put(CITY, city);
     values.put(STREET, street);
     values.put(COUNTRY, country);
     return db.insert(TABLE_NAME, null, values);
}

[/sourcecode]</blockquote>

<p>Next we will implement the delete method. The delete method needs the table name, and the where clause. In our case we delete by id.
<blockquote>[sourcecode language="java"]</p>
<p>public boolean delete(long id) {
     String whereClause = ID + &quot;=&quot; + id;
     return db.delete(TABLE_NAME, whereClause, null) &gt; 0;
}</p>
<p>[/sourcecode]</blockquote>
<p style="text-align: justify;">Now we will implement the get method. It takes the id as parameter and gets by id. The columns to be extracted can be specified in a String array. It also has a where clause. The query method returns a Cursor object. The cursor.moveToFirst() method ensures that it points to the object found, if it has.</p></p>
<blockquote>[sourcecode language="java"]

public Cursor get(long id) throws SQLException {
     String[] columns = new String[] { ID, CITY, STREET, COUNTRY };
     String whereClause = ID + &quot;=&quot; + id;
     Cursor mCursor = db.query(true, TABLE_NAME, columns, whereClause, null, null, null, null, null);
     if (mCursor != null) {
          mCursor.moveToFirst();
     }
     return mCursor;
}

[/sourcecode]</blockquote>

<p style="text-align: justify;">The getAll method is same as get() other than it doesn’t has any where clause. The Cursor object returned has methods like moveToFirst(), moveToNext(), moveToLast() which can be used to iterate over the list obtained.</p>

<blockquote>[sourcecode language="java"]

public Cursor getAll() {
     String[] columns = new String[] { ID, CITY, STREET, COUNTRY };
     return db.query(TABLE_NAME, columns, null, null, null, null, null);
}

[/sourcecode]</blockquote>

<p>The update method is same as the insert method, other than that it has a where clause.
<blockquote>[sourcecode language="java"]</p>
<p>public boolean update(long id, String city, String street, String country) {
     ContentValues values = new ContentValues();
     values.put(CITY, city);
     values.put(STREET, street);
     values.put(COUNTRY, country);
     String whereClause = ID + &quot;=&quot; + id;
     return db.update(TABLE_NAME, values, whereClause, null) &gt; 0;
}</p>
<p>[/sourcecode]</blockquote>
To test this code you can create an Activity which uses its onCreate() to initialize the AddressDao. Then insert, get and delete an object using the AddressDAO.
<blockquote>[sourcecode language="java"]</p>
<p>public class AddressActivity extends Activity {</p>
<p>@Override
public void onCreate(Bundle savedInstanceState) {
     super.onCreate(savedInstanceState);
     setContentView(R.layout.main);
     AddressDAO addressDAO = new AddressDAO(this);
     insert(addressDAO);
     get(addressDAO);
     delete(addressDAO);
}</p>
<p>private void insert(AddressDAO addressDAO) {
     addressDAO.open();
     long id = addressDAO.insert(&quot;Gurgaon&quot;, &quot;Sector 14&quot;, &quot;India&quot;);
     addressDAO.close();
}</p>
<p>private void get(AddressDAO addressDAO) {
     addressDAO.open();
     Cursor c = addressDAO.get(1);
     if (c.moveToFirst())
          DisplayAddress(c);
     else
          Toast.makeText(this, &quot;No address found&quot;, Toast.LENGTH_LONG).show();
     addressDAO.close();
}</p>
<p>private void delete(AddressDAO addressDAO) {
     addressDAO.open();
     if (addressDAO.delete(1))
          Toast.makeText(this, &quot;Delete successful.&quot;, Toast.LENGTH_LONG).show();
     else
          Toast.makeText(this, &quot;Delete failed.&quot;, Toast.LENGTH_LONG).show();</p>
<pre><code> addressDAO.close();
</code></pre>
<p>}</p>

## Comments

**[Sandeep](#3070 "2010-10-27 17:02:53"):** * public boolean onCreate() { 10 DatabaseHelper dbHelper = new DatabaseHelper(getContext()); 11 db = dbHelper.getWritableDatabase(); 12 return (db == null) ? false : true; 13 } */ Change the line number 11. call dbHelper.open() instead of dbHelper.getWritableDatabase()

