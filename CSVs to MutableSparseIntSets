package bitsetTest;
import java.util.Hashtable;
import java.util.Map;
import com.opencsv.CSVReader;

import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.IOException;
import java.util.ArrayList;

import com.ibm.wala.util.intset.MutableSparseIntSet;
import com.ibm.wala.util.intset.MutableSparseIntSetFactory;

public class TestBitset {

	public static void main (String[] arr)
	{
		ArrayList<String> toCompare = new ArrayList<String>();
    //fill toCompare with tables to be turned into MutableSparseIntSets and compared /
		vectorize(to compare);
	}
	
	private static void vectorize(ArrayList<String> table) {
		final long startTime = System.nanoTime();
		Map<String,Integer> universalColumns = new Hashtable<String,Integer>(); 
		for (int i = 0; i < table.size(); i++) 
		{
			String fileName = table.get(i);
			CSVReader reader = null;
			try {
				reader = new CSVReader(new FileReader(fileName));
			} catch (FileNotFoundException e1) {
				// TODO Auto-generated catch block
				e1.printStackTrace();
			}
			String[] header = null;
			try {
				header = reader.readNext();
			} catch (IOException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
			for (int j = 0; j < header.length; j++)
			{
				if (! universalColumns.containsKey(header[j]) ) 
				{
					universalColumns.put(header[j],universalColumns.size());
				}
			}
			try {
				reader.close();
			} catch (IOException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
		}
		
		ArrayList<MutableSparseIntSet> allIntSets = new ArrayList<MutableSparseIntSet>();
		for (int i = 0; i < table.size(); i++) 
		{
			MutableSparseIntSetFactory factory = new MutableSparseIntSetFactory();
			MutableSparseIntSet bitset = factory.make();
			String fileName = table.get(i);
			CSVReader reader = null;
			try {
				reader = new CSVReader(new FileReader(fileName));
			} catch (FileNotFoundException e1) {
				// TODO Auto-generated catch block
				e1.printStackTrace();
			}
			String[] header = null;
			try {
				header = reader.readNext();
			} catch (IOException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
			for (int j = 0; j < header.length; j++)
			{
				bitset.add(universalColumns.get(header[j]));
				//System.out.println(universalColumns.get(header[j]));
			}
			
			allIntSets.add(bitset);
			//System.out.println(bitset);
		}
		final long endTime = System.nanoTime();
		double seconds = (double)(endTime-startTime) / 1000000000.0;
		System.out.println(allIntSets.size() + ", " + universalColumns.size());
		System.out.println(seconds);
		System.out.println();
	}
	
}


