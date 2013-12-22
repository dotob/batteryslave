---
layout: post
title: "python interop: using jython and ironruby"
date: 2010-11-30 21:13
comments: true
---
in a project with java and .net komponents we needed to share some validation-code. this code should be executable on both platforms and act against pocos (plain old clr objects) and pojos (plain old java objects). 
we tested python as a language that can be executed on both sides using jython for java and ironpython for c#. i want to show you how simple an integration of python is here. 
for c# and ironpython:

``` csharp
	using System;
	using IronPython.Hosting;
	using Microsoft.Scripting;
	using Microsoft.Scripting.Hosting;

	namespace IronPythonValidationTest
	{  
		internal class Program
		{    
			private static void Main(string[] args)
			{      
				string code = @"if this.Von > this.Bis:    
								result = 'von soll kleiner sein als bis'elif this.Child.Name != '':    
								result = 'name soll gefüllt sein'";      
				ScriptEngine engine = Python.CreateEngine ();      
				ScriptScope scope = engine.CreateScope ();      
				scope.SetVariable ("this", new TestType {Von = DateTime.Now.AddDays(1), Bis = DateTime.Now, Child = new TestTypeNested()});      
				ScriptSource source = engine.CreateScriptSourceFromString (code, SourceCodeKind.SingleStatement);      
				source.Execute (scope);      
				string validationResult = scope.GetVariable ("result");      
				Console.WriteLine (validationResult);      
				Console.ReadKey ();
			}
		}

		public class TestType
		{    
			public string Name { get; set; }
			public DateTime Von { get; set; }
			public DateTime Bis { get; set; }
			public TestTypeNested Child { get; set; }
		}

		public class TestTypeNested
		{    
			public string NochName { get; set; }
		}
	}
```

and almost the same for java:

``` java
	import org.python.util.PythonInterpreter;
	import org.python.core.*;
	public class JythonValidationTest {
		public static void main(String[] args) {
			PythonInterpreter python = new PythonInterpreter();
	
			String code = "if this.Von > this.Bis:\n    result = \"von soll kleiner sein als bis\"\nelif this.Child.Name != \"\":\n    result = \"name soll gefüllt sein\"";
	
			PyCode pc = python.compile(code);
			TestType l = new TestType();
			l.setVon(2);        
			l.setBis(1);
			python.set("this", l);
		}
	}
	
	// this has to be a public class, so one could not put it in one classfile
	public class TestType {    
		public int von;    
		public String name;    
		public int bis;    
		public String getName() {        
			return name;    
		}    
		public void setName(String name) {        
			this.name = name;    
		}    
		public int getVon() {        
			return von;    
		}    
		public void setVon(int von) {        
			this.von = von;    
		}    
		public int getBis() {        
			return bis;    
		}    
		public void setBis(int bis) {        
			this.bis = bis;    
		}
	}
```

remaining problems:

- because of naming-convention differences you have to wrap python code so that you can intersept calls to get*() methods. as in c# getters and setters look like Name instead of getName()
- if there are user visible strings involved, you have to think about translating them
- checking the python code before deployment
 