#cd/opt
const tonweb = new TonWeb();

const wallet = tonweb.wallet.create({publicKey});

const address = await wallet.getAddress();

const nonBounceableAddress = address.toString(true, true, false);

const seqno = await wallet.methods.seqno().call();

await wallet.deploy(secretKey).send(); // deploy wallet to blockchain

const fee = await wallet.methods.transfer({
    secretKey,
    toAddress: 'EQDjVXa_oltdBP64Nc__p397xLCvGm2IcZ1ba7anSW0NAkeP',
    amount: TonWeb.utils.toNano(0.01), // 0.01 TON
    seqno: seqno,
    payload: 'Hello',
    sendMode: 3,
}).estimateFee();

const Cell = TonWeb.boc.Cell;
const cell = new Cell();
cell.bits.writeUint(0, 32);
cell.bits.writeAddress(address);
cell.bits.writeGrams(1);
console.log(cell.print()); // print cell data like Fift
const bocBytes = cell.toBoc();

const history = await tonweb.getTransactions(address);

const balance = await tonweb.getBalance(address);

tonweb.sendBoc(bocBytes);
