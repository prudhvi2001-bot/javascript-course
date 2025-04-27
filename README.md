'use strict';

/////////////////////////////////////////////////
/////////////////////////////////////////////////
// BANKIST APP

// Data
const account1 = {
  owner: 'Jonas Schmedtmann',
  movements: [200, 450, -400, 3000, -650, -130, 70, 1300],
  interestRate: 1.2, // %
  pin: 1111,
};

const account2 = {
  owner: 'Jessica Davis',
  movements: [5000, 3400, -150, -790, -3210, -1000, 8500, -30],
  interestRate: 1.5,
  pin: 2222,
};

const account3 = {
  owner: 'Steven Thomas Williams',
  movements: [200, -200, 340, -300, -20, 50, 400, -460],
  interestRate: 0.7,
  pin: 3333,
};

const account4 = {
  owner: 'Sarah Smith',
  movements: [430, 1000, 700, 50, 90],
  interestRate: 1,
  pin: 4444,
};

const accounts = [account1, account2, account3, account4];

// Elements
const labelWelcome = document.querySelector('.welcome');
const labelDate = document.querySelector('.date');
const labelBalance = document.querySelector('.balance__value');
const labelSumIn = document.querySelector('.summary__value--in');
const labelSumOut = document.querySelector('.summary__value--out');
const labelSumInterest = document.querySelector('.summary__value--interest');
const labelTimer = document.querySelector('.timer');

const containerApp = document.querySelector('.app');
const containerMovements = document.querySelector('.movements');

const btnLogin = document.querySelector('.login__btn');
const btnTransfer = document.querySelector('.form__btn--transfer');
const btnLoan = document.querySelector('.form__btn--loan');
const btnClose = document.querySelector('.form__btn--close');
const btnSort = document.querySelector('.btn--sort');

const inputLoginUsername = document.querySelector('.login__input--user');
const inputLoginPin = document.querySelector('.login__input--pin');
const inputTransferTo = document.querySelector('.form__input--to');
const inputTransferAmount = document.querySelector('.form__input--amount');
const inputLoanAmount = document.querySelector('.form__input--loan-amount');
const inputCloseUsername = document.querySelector('.form__input--user');
const inputClosePin = document.querySelector('.form__input--pin');


const displayMovements = function(movements){
  containerMovements.innerHTML = '';
  movements.forEach(function(mov, i){
    const type = mov>0? 'deposit':'withdrawal'
    const html = `
    <div class="movements__row">
          <div class="movements__type movements__type--${type}">${i+1} ${type}</div>
          <div class="movements__value">${mov}€</div>
        </div>`;
        containerMovements.insertAdjacentHTML("afterbegin",html);
  });

}

// displayMovements(account1.movements);

// Creating User Names for the account holders based on there usernames
const createUserNames = function(accs){
  accs.forEach(function(acc){
    acc.username = acc.owner.toLowerCase().split(' ').map(name => name[0]).join('');
  }); 
};

createUserNames(accounts);

// Calculating total Balance in account (Deposit + Withdraw)
const calcDisplayBalance = function(movements){
  const balance = movements.reduce((acc, mov) => acc +mov, 0);
  labelBalance.textContent = `${balance} EUR`;
};

// calcDisplayBalance(account1.movements);


// Calculating the Display Summary
const calDisplaySummary  = function(acc){
const incomes = acc.movements.filter(mov => mov >0).reduce((acc, cur) => acc + cur, 0)
labelSumIn.textContent = `${incomes}$`

const out = acc.movements.filter(mov => mov < 0).reduce((acc, cur)=> acc+cur,0);
labelSumOut.textContent = Math.abs(out);

const intrest = acc.movements.filter(mov => mov > 0).map(desposit => (desposit * acc.interestRate)/100).filter(int => int>1).reduce((acc,deposit) => acc+deposit, 0);
labelSumInterest.textContent = `${intrest}`
}


// calDisplaySummary(account1.movements)


// login event Handling

let currentAccount;
btnLogin.addEventListener('click', function(e){

  e.preventDefault();

currentAccount = accounts.find(acc => acc.username === inputLoginUsername.value);

if(currentAccount?.pin === Number(inputLoginPin.value)){
// Display UI and Welcome Message
labelWelcome.textContent = `Welcome back ${currentAccount.owner.split(' ')[0]}`;
containerApp.style.opacity = 1;
// clear the input filds
inputLoginUsername.value = inputLoginPin.value = '';
inputLoginPin.blur();

// Display movements
displayMovements(currentAccount.movements);
// Display balance
calcDisplayBalance(currentAccount.movements);
// Display Summary
calDisplaySummary(currentAccount);
}
})

/////////////////////////////////////////////////
/////////////////////////////////////////////////
// LECTURES

const currencies = new Map([
  ['USD', 'United States dollar'],  
  ['EUR', 'Euro'],
  ['GBP', 'Pound sterling'],
]);

const movements = [200, 450, -400, 3000, -650, -130, 70, 1300];

/////////////////////////////////////////////////

const euroToUsd = 1.1;


// const movementsUSD = movements.map(function(mov){
// return mov*euroToUsd;
// });

// const movementsUSD = movements.map((mov) => mov*euroToUsd);

// const movementsUSDfor = []
// for(const mov of movements){
//   movementsUSDfor.push(mov*euroToUsd);
//   console.log(movementsUSDfor);
// }

// const movementDescription = movements.map((mov, i) =>{
//  `Movement ${i+1}: You ${mov > 0 ? 'deposits': 'Withdrew'} ${Math.abs(mov)}`
// })

// maximum values 

// const max = movements.reduce((acc, curr, i) =>{
// if(acc>mov)return acc;
// else return mov;

// },movements[0]);
// console.log(max);

movements.filter(mov => mov > 0).map(mov => mov * euroToUsd).reduce((acc, mov) => acc+mov, 0);
