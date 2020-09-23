<div align="center">

## PHP Mine


</div>

### Description

PHP version of famous "Minesweeper" game by Microsoft by Mathias Daval
 
### More Info
 


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[PHP Code Exchange](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/php-code-exchange.md)
**Level**          |Intermediate
**User Rating**    |3.2 (16 globes from 5 users)
**Compatibility**  |PHP 3\.0, PHP 4\.0
**Category**       |[Games](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/games__8-13.md)
**World**          |[PHP](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/php.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/php-code-exchange-php-mine__8-183/archive/master.zip)





### Source Code

```
<?
/*/////// PHPMINE v1.0
/////////
// Copyright 2000, Kidou / PHPVault (http://www.phpvault.com)
//////////////////////////////////////////////////////////////////////////
// Ce script est un freeware. Vous pouvez l'utiliser et le modifier librement, mais vous devez laisser le copyright. Merci de me faire savoir si vous trouvez ce script utile. (mathias@phpvault.com)
// PHPMine est une version PHP du célèbre jeu "Démineur" de Mirosoft.
/////////////////////////////////////////////////////////////////////////
// This script is a freeware. You can use it and modify it freely as long as you leave the copyright. Please tell me if you find this script useful (mathias@phpvault.com)
// PHPMine is a PHP version of famous game "Minesweeper" by Microsoft.
/////////////////////////////////////////////////////////////////////////*/
print "<html>";
print "<head>";
print "<title>PHPVault's PHPMine v1.0</title>";
print "</head>";
print "<body><center>";
print "<font size=4 face=Verdana><b>PHPMine v1.0</b>";
if ($submit=="") {
   $NumMine=4;
   $RowSize=5;
   $ColSize=5;
   $generer=1;
}
if ($generer==1) {
   srand((double)microtime()*100000000);
   $time_start=time();
   if (($RowSize<=1) || ($ColSize<=1) || ($NumMine==0)) {
     print "<p><br><font size=-1 color=red>Wrong number for Rows, Cols or Mines!</font>";
     exit;
   }
   if ($NumMine > $RowSize*$ColSize) {
     print "<p><br><font size=-1 color=red>Too many mines!</font>";
     exit;
   }
   for ($Row=1;$Row<=$RowSize;$Row++) {
     for ($Col=1;$Col<=$ColSize;$Col++) {
       $Mine[$Row][$Col]="0";
       $Decouv[$Row][$Col]="0";
     }
   }
   $index=0;
   while ($index<$NumMine) {
     $Row=rand(1,$RowSize);
     $Col=rand(1,$ColSize);
     if ($Mine[$Row][$Col]=="0") {
       $Mine[$Row][$Col]="1";
       $index++;
     }
   }
} else {
   $perdu=0;
   $reste=$RowSize*$ColSize;
   for ($Row=1;$Row<=$RowSize;$Row++) {
     for ($Col=1;$Col<=$ColSize;$Col++) {
       $temp="Mine".($Row*($ColSize+1)+$Col);
       $Mine[$Row][$Col]=$$temp;
       $temp="Decouv".($Row*($ColSize+1)+$Col);
       $Decouv[$Row][$Col]=$$temp;
     if ($Decouv[$Row][$Col]=="1") {$reste=$reste-1;}
       $temp="submit".($Row*($ColSize+1)+$Col);
       if ($$temp=="ok") {
         $reste=$reste-1;
         if ($Mine[$Row][$Col]=="0") {
           $Decouv[$Row][$Col]="1";
         } else {
           $perdu=1;
         }
       }
     }
   }
   if ($perdu==1) {
     print "<h2>You Lose!</h2>";
     for ($i=1;$i<=$RowSize;$i++) {
       for ($j=1;$j<=$ColSize;$j++) {
         $Decouv[$i][$j]="1";
       }
     }
   }
   if (($reste==$NumMine)&&($perdu!=1)) {
     print "<h2>You Win!</h2>";
     $time_stop=time();
     $time=$time_stop-$time_start;
     print "<p><font size=-1><i>Your score: $time</i></font>";
     for ($i=1;$i<=$RowSize;$i++) {
       for ($j=1;$j<=$ColSize;$j++) {
         $Decouv[$i][$j]="1";
       }
     }
   }
}
print "<form method=get action=\"$PHP_SELF\">";
print "<input type=hidden name=time_start value=$time_start>";
print "<input type=hidden name=NumMine value=$NumMine>";
print "<input type=hidden name=RowSize value=$RowSize>";
print "<input type=hidden name=ColSize value=$ColSize>";
print "<input type=hidden name=generer value=0>";
print "<p><table border=1 cellpadding=8>";
for ($Row=1; $Row<=$RowSize; $Row++) {
   print "<tr>";
   for ($Col=1; $Col<=$ColSize; $Col++) {
     $nb=0;
     for ($i=-1; $i<=1; $i++) {
       for ($j=-1; $j<=1; $j++) {
         if ($Mine[$Row+$i][$Col+$j] == "1") {
           $nb++;
         }
       }
     }
     print "<td width=15 height=15 align=center valign=middle>";
     if ($Decouv[$Row][$Col]=="1") {
       if ($nb==0) {
         print "&nbsp;";
       } else {
         if ($Mine[$Row][$Col]=="1") {
           print "*";
         } else {
           print "$nb";
         }
       }
     } else {
       print "<input type=hidden name=submit value=ok>";
       print "<input type=submit name=submit".($Row*($ColSize+1)+$Col)." value=ok>";
     }
     print "<input type=hidden name=Mine".($Row*($ColSize+1)+$Col)." value=".$Mine[$Row][$Col].">";
     print "<input type=hidden name=Decouv".($Row*($ColSize+1)+$Col)." value=".$Decouv[$Row][$Col].">";
     print "</td>";
   }
   print "</tr>";
}
print "</table>";
print "</form>";
?>
<hr>
<form method=post>
Rows : &nbsp;<input type=text name=RowSize value=5 size=2>
<br>Cols : &nbsp;<input type=text name=ColSize value=5 size=2>
<br>Mines : &nbsp;<input type=text name=NumMine value=4 size=2>
<p><input type=submit name=submit value=Generate>
<input type=hidden name=generer value=1>
</form>
<p>
<center><font size=-2>(c) 2000, <a href="http://www.phpvault.com">PHPVault</a> - All rights reserved<br>Script by <a href="mailto:perso@kidou.net">Kidou</a>
</font>
</center>
</body>
</html>
```

