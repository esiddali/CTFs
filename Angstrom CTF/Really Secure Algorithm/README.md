# Really Secure Algorithm

## Challenge

"I found this flag somewhere when I was taking a walk, but it seems to have been encrypted with this [Really Secure Algorithm](Really_Secure_Algorithm.txt)!"

## Process

By opening up the text file given in the problem, we can determine that this problem is an RSA encryption problem because we are given p, q, e, and c values.

I wrote [this](RSA_Breaker.py) python program in order to decrypt the flag.
```
p = 8337989838551614633430029371803892077156162494012474856684174381868510024755832450406936717727195184311114937042673575494843631977970586746618123352329889
q = 7755060911995462151580541927524289685569492828780752345560845093073545403776129013139174889414744570087561926915046519199304042166351530778365529171009493
e = 65537
c = 7022848098469230958320047471938217952907600532361296142412318653611729265921488278588086423574875352145477376594391159805651080223698576708934993951618464460109422377329972737876060167903857613763294932326619266281725900497427458047861973153012506595691389361443123047595975834017549312356282859235890330349
n = p*q

#I did not write the egcd and modinv functions, I found them on the internet
def egcd(a, b):
    if a ==0:
        return (b,0,1)
    else:
        g, y, x = egcd(b % a, a)
        return (g, x - (b//a) * y, y)

def modinv(a, m):
    g, x, y = egcd(a, m)
    if g != 1:
        raise Exception('modular inverse does not exist')
    else:
        return x % m

#In order to do the decryption math below I followed the wikipedia article on RSA encryption
phi = (p-1)*(q-1)
d = modinv(e, phi)
m = hex(pow(c,d,n))[2:]
#I cut the last character off m, which is L, in order to decode it into ascii
m = m[:-1]
flag = m.strip().decode('hex')
print flag
#The flag is actf{really_securent_algorithm}
```

The flag is actf{really_securent_algorithm}