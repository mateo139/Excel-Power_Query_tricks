# 1 step - record steps in Power Query

![image](https://user-images.githubusercontent.com/103432222/226887311-59863757-e17f-48b7-9dc6-d68c779871cc.png)

# 2 step - use "Advanced editor"

![image](https://user-images.githubusercontent.com/103432222/226887561-14268071-7772-49d8-a4e4-4bd847425a21.png)

# 3 step - modify source name/path => copy script (in M language)

```
let
    Źródło = Excel.CurrentWorkbook(){[Name="source2020"]}[Content],
    #"Zmieniono typ" = Table.TransformColumnTypes(Źródło,{{"Kod towaru", type text}, {"Kolumna1", type any}, {"nazwa towaru", type text}, {"Kolumna2", type any}, {"jm", type text}, {"Kolumna3", type any}, {"ilość +", type number}, {"przychód", type number}, {"ilość -", type number}, {"rozchód", type number}}),
    #"Usunięto kolumny" = Table.RemoveColumns(#"Zmieniono typ",{"Kolumna1", "Kolumna2", "Kolumna3"}),
    #"Przefiltrowano wiersze" = Table.SelectRows(#"Usunięto kolumny", each ([jm] = "szt" or [jm] = "[szt]")),
    #"Usunięto inne kolumny" = Table.SelectColumns(#"Przefiltrowano wiersze",{"Kod towaru", "jm", "ilość -"}),
    #"Przefiltrowano wiersze1" = Table.SelectRows(#"Usunięto inne kolumny", each not Text.Contains([Kod towaru], "Trapez")),
    #"Przefiltrowano wiersze2" = Table.SelectRows(#"Przefiltrowano wiersze1", each not Text.Contains([Kod towaru], "trapez")),
    #"Zamieniono wartość" = Table.ReplaceValue(#"Przefiltrowano wiersze2","Comp ","",Replacer.ReplaceText,{"Kod towaru"}),
    #"Zamieniono wartość1" = Table.ReplaceValue(#"Zamieniono wartość","Cal ","",Replacer.ReplaceText,{"Kod towaru"}),
    #"Zamieniono wartość2" = Table.ReplaceValue(#"Zamieniono wartość1","OBn ","",Replacer.ReplaceText,{"Kod towaru"}),
    #"Zamieniono wartość3" = Table.ReplaceValue(#"Zamieniono wartość2","-o","",Replacer.ReplaceText,{"Kod towaru"}),
    #"Przefiltrowano wiersze3" = Table.SelectRows(#"Zamieniono wartość3", each not Text.Contains([Kod towaru], "Kern")),
    #"Przefiltrowano wiersze4" = Table.SelectRows(#"Przefiltrowano wiersze3", each not Text.Contains([Kod towaru], "Q")),
    #"Przefiltrowano wiersze5" = Table.SelectRows(#"Przefiltrowano wiersze4", each not Text.Contains([Kod towaru], "PerfZ")),
    #"Podzielono kolumnę według ogranicznika" = Table.SplitColumn(#"Przefiltrowano wiersze5", "Kod towaru", Splitter.SplitTextByDelimiter("x", QuoteStyle.Csv), {"Kod towaru.1", "Kod towaru.2", "Kod towaru.3"}),
    #"Zmieniono typ1" = Table.TransformColumnTypes(#"Podzielono kolumnę według ogranicznika",{{"Kod towaru.1", type text}, {"Kod towaru.2", Int64.Type}, {"Kod towaru.3", Int64.Type}}),
    #"Podzielono kolumnę według ogranicznika1" = Table.SplitColumn(#"Zmieniono typ1", "Kod towaru.1", Splitter.SplitTextByDelimiter(" ", QuoteStyle.Csv), {"Kod towaru.1.1", "Kod towaru.1.2"}),
    #"Zmieniono typ2" = Table.TransformColumnTypes(#"Podzielono kolumnę według ogranicznika1",{{"Kod towaru.1.1", type text}, {"Kod towaru.1.2", Int64.Type}}),
    #"Zmieniono kolejność kolumn" = Table.ReorderColumns(#"Zmieniono typ2",{"Kod towaru.1.1", "Kod towaru.1.2", "Kod towaru.2", "Kod towaru.3", "ilość -", "jm"})
in
    #"Zmieniono kolejność kolumn"
 ```
 # 4 step - paste script in to the new query and voila...
 
 ![image](https://user-images.githubusercontent.com/103432222/226889630-a725d069-3e7d-4a4b-8759-69225792c790.png)

