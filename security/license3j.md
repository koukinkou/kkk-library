# 使用License3j实现简单的License验证
## 借助git生成密钥
- License3j使用的是PGP密钥，但本身不提供密钥对生成功能，你可以借助其他工具，如果你安装过Git，那么Git的安装目录下已经有一个可以直接使用的GPG程序，可以执行如下来生成一对密钥：
- 启动Git Bash
- 一旦完成了这个向导，就获得了一对用于加密和签名的密钥。
- 公钥、私钥都可以加密，也都可以解密。 其中：用公钥加密需要私钥解密，称为“加密”。 由于私钥是不公开的，确保了内容的保密，没有私钥无法获得内容；用私钥加密需要公钥解密，称为“签名”。

```sh
gpg --gen-key

Please select what kind of key you want:
   (1) RSA and RSA (default)
   (2) DSA and Elgamal
   (3) DSA (sign only)
   (4) RSA (sign only)
Your selection?
选择加密算法，默认选择第一个选项即可，表示加密和签名都使用 RSA 算法。(1、4都行)

RSA keys may be between 1024 and 4096 bits long.
What keysize do you want? (2048)
选择密钥长度，默认为 2048，建议输入 4096。

Please specify how long the key should be valid.
      0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0)
设定密钥的有效期。(密钥只是个人使用的话，建议选择第一个选项，即永不过期。)

Is this correct? (y/N)
系统会问你上述设置是否正确。

You need a user ID to identify your key; the software constructs the user ID
from the Real Name, Comment and Email Address in this form:
    "Guangzhou keyun bigdata corp <yi.zhou@huidayun.com>"

Real name:填入你的名字，需是英文。回车。
Email address:填入你的邮箱地址。回车。
Comment:可以空着不填。回车。 

You selected this USER-ID:
    "Teddysun <i@teddysun.com>"

Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit?
输入 O，回车。

You need a Passphrase to protect your secret key.
Enter passphrase:
Repeat passphrase:
系统会让你设定一个私钥的密码。



gpg --list-keys

/c/Users/test/.gnupg/pubring.gpg
--------------------------------
pub   4096R/D06332E6 2017-09-22 [expires: 2017-12-21]
uid                  Guangzhou keyun bigdata corp <yi.zhou@huidayun.com>

请注意上面的字符串"D06332E6"，这是"用户ID"的Hash字符串，可以用来替代"用户ID"。

第一行显示公钥文件名（pubring.gpg）
第二行显示公钥特征（4096 位，Hash 字符串和生成时间）
第三行显示用户信息

第四行显示公钥的subkey (如果不是默认加密算法，可能没有这行)

gpg -a -o "pubkey.txt" --export 'Guangzhou keyun bigdata corp'
默认会导出至用户目录 C:\Users\<用户名>\ 下。

如果你了解非对称加密/解密，可能已经知道这其中的原理。我们使用一对私钥公钥，私钥用于加密License原文，然后我们自己保留私钥，将公钥和加密后的内容发布，而公钥用于解密，加密后的内容只用于读取不可修改，这也算是很简单的一种实现了。

	@Test
	public void testLicense()throws Exception{
		File licenseFile = new File("d:\\idec.license");
        
        if (!licenseFile.exists()) {
        // license 文件生成
            OutputStream os = new FileOutputStream("d:\\idec.license");
            os.write(new License()
                // license 的原文
                .setLicense(new File("d:\\license-plain.txt"))
                // 私钥与之前生成密钥时产生的USER-ID
                .loadKey("d:\\secring.gpg","Guangzhou keyun bigdata corp <yi.zhou@huidayun.com>")
                // 生成密钥时输入的密码
                .encodeLicense("huidayun").getBytes("utf-8"));
            os.close();
        } else {
        // licence 文件验证
            License license = new License();
            System.out.println(new Date());
            if (license.loadKeyRing("d:\\pubring.gpg", null).setLicenseEncodedFromFile("d:\\idec.license").isVerified()) {
                System.out.println(license.getFeature("edition"));
                System.out.println(license.getFeature("valid-until"));
            }else{
            	System.out.println("过期");
            }
        
        }
	}
```