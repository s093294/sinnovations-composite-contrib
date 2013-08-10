sinnovations-composite-contrib
==============================


DATA
====
Heres an example for the AutoMapper. 
It builds a lambda expression for all matching properties with same name and type on the first query, and reuses this in the future.

Its the initial commit. Planning to add some attributes that you can put on the Static Data types for configuring the mapping rules.

```
  IQueryable<MyC1DataType> data = new MyC1DataType[] { new HiddenImp("Hej"), new HiddenImp("med"), new HiddenImp("Dig") }.AsQueryable();
  IQueryable<MyClass> mappedData = data.Map().To<MyClass>();
  IQueryable<MyClass> mappedData1 = data.Map().To<MyClass>();
  
  for (int i = 0; i < data.Count(); i++)
  {
      Assert.AreEqual(data.ElementAt(i).PropertyTwo, mappedData.ElementAt(i).PropertyTwo);
  }  
```

C1MappingRepository
-------------------

```
        ///Create a Repository for the C1 Datatype ContentSpot and map it to MyCustomContentSpot
        var test = new C1Repository<ContentSpot, MyCustomContentSpot>();
        
        ///Get all ContentSpots as MyCustomContentSpots and Delete them all.
        test.Remove(test.GetAll());


        ///Add two new ContentSpots, from mapped objects.
        var item = new MyCustomContentSpot { 
            Content = "<div> Hello World - " + Path.GetRandomFileName() + " </div>", 
            Id = Guid.NewGuid(), 
            Title = "nej med jer" ,
            PublicationStatus = "published"
        };
        test.Add(item);

        var item1 = new MyCustomContentSpot
        {
            Content = "<div> Hello World - " + Path.GetRandomFileName() + " </div>",
            Id = Guid.NewGuid(),
            Title = "nej med jer"
        };
        test.Add(item1);
       
        /// Added to Data Store
        /// 
        ///<ContentSpot Id="8c155c3e-250c-4fd8-91dc-d5b7a1f50801" Title="nej med jer"
        ///    Content="&lt;div&gt; Hello World - inaelq5k.i0n &lt;/div&gt;" PublicationStatus="draft" />
        ///<ContentSpot Id="935de270-7f7a-4686-b39a-ede8f8d1b60e" Title="nej med jer" 
        ///    Content="&lt;div&gt; Hello World - chl2q1tm.gs3 &lt;/div&gt;" PublicationStatus="published" />

        var AllContentSpots = test.GetAll(); //IQueryable<MyCustomContentSpot>
        var askDataStoreAgain = test.Get(AllContentSpots.Last()); //MyCustomContentSpot
        
```
