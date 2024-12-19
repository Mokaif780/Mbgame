# Mbgame 
  const gameResult = (seed, salt) => {
    const nBits = 52; // number of most significant bits to use
 
    // 1. HMAC_SHA256(message=seed, key=salt) 
    const hmac = CryptoJS.HmacSHA256(CryptoJS.enc.Hex.parse(seed), salt);
    seed = hmac.toString(CryptoJS.enc.Hex);
 
    // 2. r = 52 most significant bits
    seed = seed.slice(0, nBits / 4);
    const r = parseInt(seed, 16);
 
    // 3. X = r / 2^52
    let X = r / Math.pow(2, nBits); // uniformly distributed in [0; 1)
    X = parseFloat(X.toPrecision(9));
 
    // 4. X = 99 / (1-X)
    X = 99 / (1 - X);
 
    // 5. return max(trunc(X), 100)
    const result = Math.floor(X);
    return Math.max(1, result / 100);
  };
 

Prior to being used for calculation, each game hash is salted with the lowercase + hexadecimal string representation of the hash from pre-selected Bitcoin block 758,160. This block has not been mined yet as of this post, proving that we have not deliberately selected a mined block with a hash that could be favorable to the house. Once block 758,160 has been mined, the results will be posted to this thread as a reply
