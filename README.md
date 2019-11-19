# Kabylie
/*
Autho

write a program called transcriptase.cpp that reads a text file called dna.txt that contains one DNA strand per line,
. . . 
and outputs to the console (terminal) the corresponding mRNA strands. Each output line must contain exactly one mRNA strand. This is a sample output of the program:
 
Recall that to read from a file, the following code snipet can be used:
*/

#include<iostream>
#include <cstdlib>
#include <fstream>
#include <string>
using namespace std;


// this function replaces the letter of a char with another letter.
char DNAbase_to_mRNAbase(char v){
        v = toupper(v);
        if (v == 'A'){return 'U';}
        else if( v == 'T'){return 'A';}
        else if(v == 'C'){return 'G';}
        else if (v == 'G'){return 'C';}
        return v;
}


string DNA_to_mRNA(string s){ // this goes through the program
    string save = "";// empty string

    for (int i = 0; i < s.length();i++){
      // this is the same as save = save + DNAbase_to_mRNAbase(s[i]);
      save += DNAbase_to_mRNAbase(s[i]);
    }
    return save;
}

//this is a function that convert
string convert(ifstream & dict, string temp){
    
     string key, value;
  dict.clear(); // reset error state
  dict.seekg(0); // return file pointer to the beginning


  while (dict >> key >> value) {
    
    if(key==temp){
    return value;
    }
    
  }
  
  return "";
    
}

int finding(string trand) {
  string temp;
  for (int i = 0; i < trand.length(); i++){
  temp = trand.substr(i,3);
    if (temp == "AUG"){
      return i;
    }
  }
  return -1;
}

string new_function(ifstream & dict,string strand){

      string save;
      save = DNA_to_mRNA(strand);
      int n = 0;
      string sum = "";
      bool temp_DNA = false;
      string temp;

      for(int i = finding(save);i < save.length();i+=3){
          temp = save.substr(i,3);

          if (temp == "UAA" || temp == "UGA" || temp == "UAG" ){
            temp_DNA = false;
            return sum;
          }

          if(temp_DNA == true){
            sum = sum + "-" + convert(dict,temp);
          }
         if (temp == "AUG"){
            temp_DNA = true;
            sum = convert(dict,temp);
         }

        if (sum[sum.length()-1]== '-'){ sum = sum.substr(0, sum.length()-1);}
      }

      return sum;
}

int main(){
ifstream dict("codons.tsv");// this open this file
  string str1, str2;
  ifstream fin("frameshift_mutations.txt");
  if (fin.fail()) {
      cerr << "File cannot be read, opened, or does not exist.\n";
      exit(1);
  }
  while(getline(fin, str1) && getline(fin, str2)){

	 string amino1 = new_function(dict,str1);
	 string amino2 = new_function(dict,str2);
    

      cout << amino1 << endl;
      cout << amino2 << endl;

  }
  fin.close();
    return 0;
}

STUDENT
Lyes Yakoubi
AUTOGRADER SCORE
90.0 / 90.0
