<template>
  <div>
    <h2>Convergence Tests</h2>
    <p>
      URL: <input v-model="url" placeholder="convecgence server url">
    </p>
    <p>
      Username / Password <input v-model="username" placeholder="username">/<input v-model="password" placeholder="password">
    </p>
    <p>
    <button @click="connectJWT">Connect with username/password</button>
    <button @click="reconnect">Reconnect</button>
    </p>
    <div>
      Reconnect-Token: {{reconnectToken}}
    </div>
    <div>
      <p v-for="(l,i) in log" :key="i">{{l}}</p>
    </div>
  </div>

</template>

<script>
import Convergence from "@convergence/convergence"
//import JwtGenerator from "@convergence/jwt-util";
export default {
  name: 'ConvergenceTest',
  props: {
    msg: String
  },
  data() {
    return {
      url: "https://data2.bentimento.com/realtime/convergence/default",
      username: 'test',
      password: 'test',
      reconnectToken: '',
      log:["Log start"]
    }
  },
  methods: {
    connectJWT() {

     /*
      var privateKey = "-----BEGIN RSA PRIVATE KEY-----\n" +
          "MIIEpAIBAAKCAQEA4755b4ugzp5AF15dVam1LtyCAk2gSwQL6sOyQCQ1w96CLM3o\n" +
          "kEmfnH7SBXSwu1kQg/R0l1WEkbivSSNlRsuO+t/SCr9wCV3iPoRiZ8KWlSSs4IrS\n" +
          "fliNdKbVPAo+NgRF/yCoq/u5daBzH4Tn6b5WhlHaYbhAU7K5XKUJKtJbbceERD7Q\n" +
          "hTyE70Juca0UM5qqsS8ftogm7MzADD2+dM2J5ehm5TYsDeOP+g0avpRgQQhT+w6T\n" +
          "qYlsrvt27g4Zt8LyZO2P3S+Kqhhtv1Fpv7OZN05N9FcwVobmsvmBIEiImTcdN4Jk\n" +
          "GOiD2C/Y+2fArldYtHKBpYTM0Os3sAJYAXx5jQIDAQABAoIBAQC7jwGJiM+NeizE\n" +
          "UhXB17r8deOoK+/n99QsqrBhPLD+t4Azj/yI3+lQQ+2JSrnGASql0aw3edls/VF2\n" +
          "hW0B6eC94Zik+1Y0kJoj7HjyhQHooESzxABox1rwB/mysajcaZaP74CZ6vyoz9Qf\n" +
          "0CACk5vEkSKtucVYaOdZUh2Do6xichDiOrGTy63dOXJc9oPGu3awdWms/DMXW1D5\n" +
          "8CSE9lr99L6pFi3ByEwZc8LdjeqYUjDbn6zWbKPU62SpZ1d5o5fhcZkac/Ro1MCc\n" +
          "HHDn0oLLixmLnMlZLu2cbHc11rykSN2Fkg+N3ZmytENv/99iucmfdCHF7Boal7vZ\n" +
          "IYlLsFyBAoGBAP1fxmLstx0NuSre4Ct7cavd+VlXUBfDC7VD5ILDiOMbe/GtJWDb\n" +
          "xCizr7yZSo423gdzffoAgT1/xGOKGZu2SGDHV9UxAw4jK+Xdpq2FV7xRXa20ZcWP\n" +
          "G/2UqaXbDg92/OGftiIFg8lRIbhS+Qn8ICGL+SnRR36Q0OgAVUaBGKvhAoGBAOYa\n" +
          "s09dmKGYfG0iqNufWj7yki8u8H6qkgDK22uB3IHZkdXexcSWvPET46Eo2zEAHGCH\n" +
          "Axn9TPLgutPAyYeYsFpt5TYwDQ3yd/AxU8ea/jd48GEAcgbbIPFYleBHF7t68epc\n" +
          "QAXM9lbpSQRzDBo/GrGHfTs6U3mPq1RwgWWBdqMtAoGAdDsrVtYgakORXgk/7LKE\n" +
          "QsNhT4quQzhGdrDect4gPY6uHP/A1d367nD/fRUVDog88k4YzEbPAtdJ0B/Z/4Rv\n" +
          "T3Yi5LnHHQUlbh1wCMnSpaDtyHGo/ifYP0ujR2Jl4NJEz3vkoVbGTePaBCSL+fif\n" +
          "BMShWH2YvobQTp1YgS9XkgECgYEA1hgHuBN8vPX051I1TErjG+h0bWQ+LYyk1gno\n" +
          "yAmG0mZN03E4D2a1G5WG2N77uobA4GseTqMoYl8IVZdtqzU8+JaBir6AlvZ/88Lh\n" +
          "kbm7SH/FIGvarFhcOVJT+N4Mzcphpc35X6xJk6nw3yvwGceXHZDTyzTEkDlFjwYy\n" +
          "zu/7I20CgYAopHb9my5iUei+DMtIMbSwc6U3vw8Jeh2dscusjVJJMiqvlNX+JevO\n" +
          "nSkdGgzgLoppEwN4xSvP850ZWkArmpcafnzfEfL3VZ5lYmzGLOIS9LxEhnI8s/gJ\n" +
          "y/hm9lGNBsYQnT/zo5fuW9piHJbNhPAEL1MyoV5wsZQztwF5tRaWlQ==\n" +
          "-----END RSA PRIVATE KEY-----"
      var keyId = "my-convergence-key";
      var gen = new JwtGenerator(keyId, privateKey);
      var claims = {firstName: "John", lastName: "Doe"};
      var username = "jdoe";
      var token = gen.generate(username, claims);
*/



      try {
        this.log.push("trying to connect to "+this.url);
        Convergence.connect(this.url, "test","test").then((domain) => {
          this.log.push("connected successfully to "+this.url+" "+domain.toString());
          this.reconnectToken = domain.session().reconnectToken();
          this.log.push("retrieved reconnect token to "+this.url+" "+this.reconnectToken.toString());
          domain.disconnect();
          this.log.push("disconnected")
        })
      } catch(e) {
        this.log.push("ERROR "+e.toString());
      }
    },
    reconnect() {
      this.log.push("trying to reconnect to "+this.url+ " with token "+this.reconnectToken);
      Convergence.reconnect(this.url,this.reconnectToken).then((domain) => {
        this.log.push("Reconnected successfully to "+this.url+" "+domain.toString());
      })
      .catch((e) => {
        this.log.push("ERROR "+e.toString());
      });
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
