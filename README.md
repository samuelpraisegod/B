<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Prop Firm Dashboard</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 0; padding: 0; }
        header { background: #333; color: white; padding: 10px; display: flex; justify-content: space-between; align-items: center; }
        #hamburger { cursor: pointer; font-size: 24px; }
        #menu { display: none; position: absolute; top: 50px; left: 10px; background: white; border: 1px solid #ddd; padding: 10px; }
        #menu ul { list-style: none; padding: 0; margin: 0; }
        #menu li { margin-bottom: 10px; cursor: pointer; }
        #main-content { max-width: 600px; margin: 20px auto; padding: 20px; }
        section { display: none; }
        #dashboard-section { display: block; }
        h2 { font-size: 18px; margin-top: 20px; }
        label { display: block; margin-bottom: 5px; }
        input, select, textarea { width: 100%; padding: 8px; margin-bottom: 10px; box-sizing: border-box; }
        textarea { height: 100px; }
        .account-options, .transaction-list { margin-bottom: 10px; }
        .share-display { font-weight: bold; }
        button { padding: 10px 20px; margin-right: 10px; cursor: pointer; }
        #pending-modal { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.5); justify-content: center; align-items: center; }
        #pending-content { background: white; padding: 20px; max-width: 500px; width: 90%; max-height: 80%; overflow-y: auto; }
        #pending-list, #all-requests-list, #recent-deposits, #recent-withdrawals { list-style: none; padding: 0; }
        #pending-list li, #all-requests-list li, #recent-deposits li, #recent-withdrawals li { margin-bottom: 10px; border-bottom: 1px solid #ddd; padding-bottom: 10px; }
        .tab-container { display: flex; margin-bottom: 20px; }
        .tab { padding: 10px 20px; cursor: pointer; border-bottom: 2px solid transparent; }
        .tab.active { border-bottom: 2px solid #333; font-weight: bold; }
        .tab-content { display: none; }
        .tab-content.active { display: block; }
        .method-buttons { display: flex; gap: 10px; margin-bottom: 10px; }
        .method-button { padding: 8px 16px; border: 1px solid #ddd; cursor: pointer; }
        .method-button.active { background: #333; color: white; }
        .confirm-deposit { background: green; color: white; }
        .submit-withdraw, .submit-managed { background: blue; color: white; }
        .instructions { background: #f0f0f0; padding: 10px; margin-bottom: 10px; }
        .qr-code { width: 100px; height: 100px; background: #ccc; margin: 10px 0; }
        .copy-button { padding: 5px 10px; font-size: 12px; }
        #bank-name-container { display: block; }
        #bank-name-custom { display: none; }
    </style>
</head>
<body>
    <header>
        <div id="hamburger">&#9776;</div>
        <h1>Prop Firm Dashboard</h1>
    </header>

<nav id="menu">
        <ul>
            <li data-section="dashboard-section">Dashboard</li>
            <li data-section="co-funding-section">Co-Funding Request</li>
            <li data-section="wallet-section">Wallet Dashboard</li>
            <li data-section="managed-account-section">Managed Account Application</li>
            <li data-section="requests-section">View All Requests</li>
        </ul>
    </nav>

  <div id="main-content">
        <!-- Dashboard Section -->
        <section id="dashboard-section">
            <h2>Welcome to the Dashboard</h2>
            <p>This is the main dashboard. Use the menu to navigate.</p>
            <h2>Recent Results</h2>
            <p>Placeholder for recent results (e.g., success rates, payouts).</p>
        </section>

  <!-- Co-Funding Section -->
 <section id="co-funding-section">
            <h1>Co-Funding Request</h1>
            <h2>Step 1: Select Prop Firm</h2>
            <select id="prop-firm">
                <option value="System/Partner Will Choose">System/Partner Will Choose</option>
                <option value="FTMO">FTMO</option>
                <option value="MyForexFunds">MyForexFunds</option>
                <option value="E8 Funding">E8 Funding</option>
                <option value="The Funded Trader">The Funded Trader</option>
            </select>
            <h2>Step 2: Select Account Size</h2>
            <div id="account-options" class="account-options"></div>
            <h2>Step 3: Challenge Rules</h2>
            <p id="challenge-rules">No specific challenge rules; system or partner will select the Prop Firm and account size.</p>
            <h2>Step 4: Co-Funding Details</h2>
            <label for="profit-split">Profit Split Ratio (e.g., 50:50 for requester:co-funder):</label>
            <input type="text" id="profit-split" value="50:50">
            <label>Your Contribution (Requester's Share):</label>
            <span id="requester-share" class="share-display">N/A</span>
            <label>Co-Funder's Contribution:</label>
            <span id="co-funder-share" class="share-display">N/A</span>
            <button id="submit-request">Submit Request</button>
        </section>

 <!-- Wallet Dashboard Section -->
   <section id="wallet-section">
            <h1>Wallet Dashboard</h1>
            <div class="tab-container">
                <div class="tab active" data-tab="deposit-tab">Deposit</div>
                <div class="tab" data-tab="withdraw-tab">Withdraw</div>
            </div>
            <div id="deposit-tab" class="tab-content active">
                <h2>Deposit Funds</h2>
                <label>Choose Method:</label>
                <div class="method-buttons">
                    <button class="method-button active" data-method="Bank">💳 Bank Transfer</button>
                    <button class="method-button" data-method="Card">💵 Card Payment</button>
                    <button class="method-button" data-method="Crypto">🔗 Crypto Wallet</button>
                </div>
                <label for="deposit-amount">Enter Amount ($):</label>
                <input type="number" id="deposit-amount" min="1" value="500">
                <label for="deposit-currency">Currency:</label>
                <select id="deposit-currency">
                    <option value="USD">USD</option>
                </select>
                <div id="deposit-details"></div>
                <p class="instructions">⚠️ Please make payment to the account/wallet shown above. Your deposit will be confirmed once payment is received.</p>
                <button id="submit-deposit" class="confirm-deposit">✅ Confirm Deposit</button>
                <h2>Recent Deposits</h2>
                <ul id="recent-deposits" class="transaction-list"></ul>
            </div>
            <div id="withdraw-tab" class="tab-content">
                <h2>Withdraw Funds</h2>
                <p><strong>Available Balance:</strong> $2,450.00</p>
                <label for="withdraw-amount">Enter Amount ($):</label>
                <input type="number" id="withdraw-amount" min="1" value="200">
                <label for="withdraw-method">Select Destination Method:</label>
                <div class="method-buttons">
                    <button class="method-button active" data-method="Bank Account">🏦 Bank Account</button>
                    <button class="method-button" data-method="Crypto Wallet">🔗 Crypto Wallet</button>
                    <button class="method-button" data-method="Mobile Money">📲 Mobile Money</button>
                </div>
                <div id="bank-name-container">
                    <label for="bank-name">Bank Name:</label>
                    <select id="bank-name">
                        <option value="GTBank">GTBank</option>
                        <option value="Zenith Bank">Zenith Bank</option>
                        <option value="First Bank">First Bank</option>
                        <option value="Access Bank">Access Bank</option>
                        <option value="Other">Other</option>
                    </select>
                    <input type="text" id="bank-name-custom" placeholder="Enter bank name">
                </div>
                <label for="withdraw-destination">Destination:</label>
                <input type="text" id="withdraw-destination" placeholder="Enter bank account or wallet address">
                <label for="withdraw-security">Security (OTP/PIN):</label>
                <input type="text" id="withdraw-security" placeholder="Enter OTP or PIN">
                <button id="submit-withdraw" class="submit-withdraw">Submit Withdrawal</button>
                <h2>Recent Withdrawals</h2>
                <ul id="recent-withdrawals" class="transaction-list"></ul>
            </div>
        </section>
<!-- Managed Trading Account Application Section -->
  <section id="managed-account-section">
            <h1>Managed Trading Account Application</h1>
            <h2>Investment Plan</h2>
            <label for="capital-size">Capital Size ($):</label>
            <input type="number" id="capital-size" min="1000" placeholder="Enter amount (min $1,000)">
            <label for="trader-type">Type of Trader:</label>
            <select id="trader-type">
                <option value="Forex">Forex</option>
                <option value="Stocks">Stocks</option>
                <option value="Crypto">Crypto</option>
                <option value="Commodities">Commodities</option>
                <option value="All">All</option>
            </select>
            <label for="return-cycle">Return Cycle:</label>
            <select id="return-cycle">
                <option value="Daily">Daily</option>
                <option value="Weekly">Weekly</option>
                <option value="Monthly">Monthly</option>
                <option value="Long-term">Long-term</option>
            </select>
            <label for="profit-target">Profit Target (%):</label>
            <input type="number" id="profit-target" min="0" max="100" placeholder="Enter percentage (0-100)">
            <label for="withdrawal-terms">Withdrawal Terms:</label>
            <input type="text" id="withdrawal-terms" placeholder="Describe withdrawal terms">
            <label for="lot-size">Lot Size:</label>
            <input type="number" id="lot-size" min="0.01" step="0.01" placeholder="Enter lot size (e.g., 0.01)">
            <label for="experience-level">Experience Level:</label>
            <select id="experience-level">
                <option value="Beginner">Beginner</option>
                <option value="Intermediate">Intermediate</option>
                <option value="Expert">Expert</option>
            </select>
            <h2>Proof of Trading Skills</h2>
            <label for="proof-experience">Proof of Experience (Certificates, Prop Firm Passes):</label>
            <input type="file" id="proof-experience" accept="image/*,application/pdf">
            <label for="social-media">Social Media Profile (Optional, e.g., Twitter/X, LinkedIn, Telegram):</label>
            <input type="text" id="social-media" placeholder="Enter profile URL">
            <label for="track-record">Track Record Upload (PDF/CSV, e.g., MT4/MT5 history):</label>
            <input type="file" id="track-record" accept=".pdf,.csv">
            <label for="trading-style">Trading Style / Short Bio:</label>
            <textarea id="trading-style" placeholder="Describe your strategy, risk management, and approach"></textarea>
            <h2>Optional Extras</h2>
            <label for="risk-tolerance">Risk Tolerance:</label>
            <select id="risk-tolerance">
                <option value="">Select (Optional)</option>
                <option value="Conservative">Conservative</option>
                <option value="Moderate">Moderate</option>
                <option value="Aggressive">Aggressive</option>
            </select>
            <label>
                <input type="checkbox" id="terms-agreement">
                I agree to the terms and conditions
            </label>
            <button id="submit-managed" class="submit-managed">Submit Application</button>
        </section>
<!-- View All Requests Section -->
        <section id="requests-section">
            <h2>All Requests</h2>
            <ul id="all-requests-list"></ul>
            <button id="view-pending">View and Accept Pending Requests</button>
        </section>
    </div>
 <!-- Pending Requests Modal -->
    <div id="pending-modal">
        <div id="pending-content">
            <h2>Pending Requests</h2>
            <ul id="pending-list"></ul>
            <button id="close-modal">Close</button>
        </div>
    </div>

 <script>
        // Prop Firm Data
        const propFirms = {
            "FTMO": {
                "accounts": [
                    { "size": "$1,000", "price": 13.0 },
                    { "size": "$2,000", "price": 17.0 },
                    { "size": "$5,000", "price": 30.0 },
                    { "size": "$10,000", "price": 155.0 },
                    { "size": "$100,000", "price": 500.0 },
                    { "size": "$200,000", "price": 1080.0 }
                ],
                "challenge_rules": "Must hit 10% profit target in 30 days (Step 1), 5% in 60 days (Step 2), max 5% daily loss, 10% max drawdown."
            },
            "MyForexFunds": {
                "accounts": [
                    { "size": "$10,000", "price": 49.0 },
                    { "size": "$50,000", "price": 299.0 },
                    { "size": "$200,000", "price": 999.0 }
                ],
                "challenge_rules": "Must hit 8% profit target in 30 days (Step 1), 5% in 60 days (Step 2), max 5% daily drawdown, 12% max drawdown."
            },
            "E8 Funding": {
                "accounts": [
                    { "size": "$25,000", "price": 228.0 },
                    { "size": "$100,000", "price": 398.0 },
                    { "size": "$400,000", "price": 998.0 }
                ],
                "challenge_rules": "Must hit 8% profit target in 30 days (Step 1), 5% in 60 days (Step 2), max 5% daily drawdown, 8% max loss."
            },
            "The Funded Trader": {
                "accounts": [
                    { "size": "$50,000", "price": 299.0 },
                    { "size": "$100,000", "price": 499.0 },
                    { "size": "$200,000", "price": 899.0 }
                ],
                "challenge_rules": "Must hit 10% profit target in 35 days (Step 1), 5% in 60 days (Step 2), max 6% daily loss, 12% max drawdown."
            },
            "System/Partner Will Choose": {
                "accounts": [{ "size": "N/A", "price": 0.0 }],
                "challenge_rules": "No specific challenge rules; system or partner will select the Prop Firm and account size."
            }
        };

        // Menu and Section Handling
        const hamburger = document.getElementById('hamburger');
        const menu = document.getElementById('menu');
        const menuItems = menu.querySelectorAll('li');
        const sections = document.querySelectorAll('section');

        hamburger.addEventListener('click', () => {
            menu.style.display = menu.style.display === 'block' ? 'none' : 'block';
        });

        menuItems.forEach(item => {
            item.addEventListener('click', () => {
                const sectionId = item.dataset.section;
                sections.forEach(sec => sec.style.display = 'none');
                document.getElementById(sectionId).style.display = 'block';
                menu.style.display = 'none';
                if (sectionId === 'requests-section') {
                    loadAllRequests();
                } else if (sectionId === 'wallet-section') {
                    updateDepositDetails();
                    updateWithdrawDetails();
                    loadRecentTransactions();
                }
            });
        });

        // Co-Funding Elements
        const firmSelect = document.getElementById('prop-firm');
        const accountOptions = document.getElementById('account-options');
        const challengeRules = document.getElementById('challenge-rules');
        const profitSplitInput = document.getElementById('profit-split');
        const requesterShare = document.getElementById('requester-share');
        const coFunderShare = document.getElementById('co-funder-share');
        const submitButton = document.getElementById('submit-request');
        const viewPendingButton = document.getElementById('view-pending');
        const pendingModal = document.getElementById('pending-modal');
        const pendingList = document.getElementById('pending-list');
        const closeModalButton = document.getElementById('close-modal');
        const allRequestsList = document.getElementById('all-requests-list');

        // Wallet Elements
        const tabs = document.querySelectorAll('.tab');
        const tabContents = document.querySelectorAll('.tab-content');
        const depositAmount = document.getElementById('deposit-amount');
        const depositCurrency = document.getElementById('deposit-currency');
        const depositDetails = document.getElementById('deposit-details');
        const submitDeposit = document.getElementById('submit-deposit');
        const withdrawAmount = document.getElementById('withdraw-amount');
        const withdrawMethodButtons = document.querySelectorAll('#withdraw-tab .method-button');
        const withdrawDestination = document.getElementById('withdraw-destination');
        const withdrawSecurity = document.getElementById('withdraw-security');
        const bankNameSelect = document.getElementById('bank-name');
        const bankNameCustom = document.getElementById('bank-name-custom');
        const bankNameContainer = document.getElementById('bank-name-container');
        const submitWithdraw = document.getElementById('submit-withdraw');
        const recentDeposits = document.getElementById('recent-deposits');
        const recentWithdrawals = document.getElementById('recent-withdrawals');
        const depositMethodButtons = document.querySelectorAll('#deposit-tab .method-button');

        // Managed Account Elements
        const capitalSize = document.getElementById('capital-size');
        const traderType = document.getElementById('trader-type');
        const returnCycle = document.getElementById('return-cycle');
        const profitTarget = document.getElementById('profit-target');
        const withdrawalTerms = document.getElementById('withdrawal-terms');
        const lotSize = document.getElementById('lot-size');
        const experienceLevel = document.getElementById('experience-level');
        const proofExperience = document.getElementById('proof-experience');
        const socialMedia = document.getElementById('social-media');
        const trackRecord = document.getElementById('track-record');
        const tradingStyle = document.getElementById('trading-style');
        const riskTolerance = document.getElementById('risk-tolerance');
        const termsAgreement = document.getElementById('terms-agreement');
        const submitManaged = document.getElementById('submit-managed');

        let selectedAccount = 'N/A';
        let accountPrice = 0.0;
        let selectedDepositMethod = 'Bank';
        let selectedWithdrawMethod = 'Bank Account';

        // Load requests from localStorage
        function loadRequests() {
            return JSON.parse(localStorage.getItem('requests')) || [];
        }

        // Save requests to localStorage
        function saveRequests(requests) {
            localStorage.setItem('requests', JSON.stringify(requests));
        }

        // Generate random reference code
        function generateReferenceCode() {
            return 'REF-' + Math.random().toString(36).substr(2, 4).toUpperCase();
        }

        // Update co-funding account sizes
        function updateAccountSizes() {
            const firm = firmSelect.value;
            accountOptions.innerHTML = '';
            selectedAccount = 'N/A';
            accountPrice = 0.0;

            propFirms[firm].accounts.forEach(acc => {
                const label = document.createElement('label');
                const radio = document.createElement('input');
                radio.type = 'radio';
                radio.name = 'account-size';
                radio.value = acc.size;
                radio.addEventListener('change', () => {
                    selectedAccount = acc.size;
                    accountPrice = acc.price;
                    calculateShares();
                });
                label.appendChild(radio);
                label.appendChild(document.createTextNode(` ${acc.size} - $${acc.price.toFixed(2)}`));
                accountOptions.appendChild(label);
            });

            challengeRules.textContent = propFirms[firm].challenge_rules;
            calculateShares();
        }

        // Calculate co-funding shares
        function calculateShares() {
            const profitSplit = profitSplitInput.value || '50:50';
            if (selectedAccount === 'N/A') {
                requesterShare.textContent = 'N/A';
                coFunderShare.textContent = 'N/A';
                return;
            }

            try {
                const [reqPart, coPart] = profitSplit.split(':').map(Number);
                const totalParts = reqPart + coPart;
                if (totalParts === 0) throw new Error();
                const reqShare = (reqPart / totalParts) * accountPrice;
                const coShare = (coPart / totalParts) * accountPrice;
                requesterShare.textContent = `$${reqShare.toFixed(2)} (Locked in Escrow)`;
                coFunderShare.textContent = `$${coShare.toFixed(2)} (Awaiting Co-Funder)`;
            } catch {
                requesterShare.textContent = 'Invalid Ratio';
                coFunderShare.textContent = 'Invalid Ratio';
            }
        }

        // Submit co-funding request
        function submitRequest() {
            const firm = firmSelect.value;
            if (firm === 'System/Partner Will Choose' || selectedAccount === 'N/A') {
                alert('Please select a valid Prop Firm and Account Size.');
                return;
            }

            const profitSplit = profitSplitInput.value || '50:50';
            if (!profitSplit.includes(':')) {
                alert('Please enter a valid profit split ratio (e.g., 50:50).');
                return;
            }

            const reqShareText = requesterShare.textContent;
            if (reqShareText.includes('Invalid')) {
                alert('Invalid share calculation.');
                return;
            }

            const reqShareVal = parseFloat(reqShareText.replace('$', '').split(' ')[0]);
            const coShareVal = parseFloat(coFunderShare.textContent.replace('$', '').split(' ')[0]);

            const requests = loadRequests();
            const newRequest = {
                id: requests.length + 1,
                type: 'co-funding',
                prop_firm: firm,
                account_size: selectedAccount,
                account_price: accountPrice,
                profit_split: profitSplit,
                requester_share: reqShareVal,
                co_funder_share: coShareVal,
                status: 'Pending',
                date: new Date().toLocaleDateString('en-US', { month: 'short', day: 'numeric', year: '2-digit' })
            };
            requests.push(newRequest);
            saveRequests(requests);

            alert(`Co-Funding Request submitted! Your contribution: $${reqShareVal.toFixed(2)} (Locked in Escrow). Waiting for a co-funder... Status: Pending`);
            profitSplitInput.value = '50:50';
            firmSelect.value = 'System/Partner Will Choose';
            updateAccountSizes();
        }

        // Update deposit payment details
        function updateDepositDetails() {
            const details = {
                'Bank': `
                    <p><strong>Payment Details:</strong></p>
                    <p>Bank Name: GTBank</p>
                    <p>Account Name: MarketFlowFX Ltd</p>
                    <p>Account Number: 0123456789</p>
                    <p>Reference Code: ${generateReferenceCode()}</p>
                    <label for="payment-proof">Upload Proof of Payment:</label>
                    <input type="file" id="payment-proof" accept="image/*,application/pdf">
                `,
                'Card': `
                    <p><strong>Payment Details:</strong></p>
                    <label for="card-number">Card Number:</label>
                    <input type="text" id="card-number" placeholder="1234-5678-9012-3456">
                    <label for="card-expiry">Expiry (MM/YY):</label>
                    <input type="text" id="card-expiry" placeholder="MM/YY">
                    <label for="card-cvv">CVV:</label>
                    <input type="text" id="card-cvv" placeholder="123">
                `,
                'Crypto': `
                    <p><strong>Payment Details:</strong></p>
                    <label for="coin-type">Select Coin:</label>
                    <select id="coin-type">
                        <option value="BTC">BTC</option>
                        <option value="USDT">USDT</option>
                        <option value="ETH">ETH</option>
                    </select>
                    <p>Wallet Address: 0xAB123456789CDEFFED1234...</p>
                    <button class="copy-button" onclick="navigator.clipboard.writeText('0xAB123456789CDEFFED1234...').then(() => alert('Address copied!'))">Copy Address</button>
                    <p>QR Code:</p>
                    <div class="qr-code"></div>
                `
            };
            depositDetails.innerHTML = details[selectedDepositMethod];
        }

        // Update withdrawal destination details
        function updateWithdrawDetails() {
            const placeholder = selectedWithdrawMethod === 'Crypto Wallet' ? 'Enter wallet address' : selectedWithdrawMethod === 'Mobile Money' ? 'Enter mobile number' : 'Enter bank account number';
            withdrawDestination.placeholder = placeholder;
            bankNameContainer.style.display = selectedWithdrawMethod === 'Bank Account' ? 'block' : 'none';
            bankNameCustom.style.display = bankNameSelect.value === 'Other' ? 'block' : 'none';
        }

        // Submit deposit request
        function submitDepositRequest() {
            const amount = parseFloat(depositAmount.value);
            if (!amount || amount <= 0) {
                alert('Please enter a valid deposit amount.');
                return;
            }

            let details = {};
            if (selectedDepositMethod === 'Bank') {
                const proof = document.getElementById('payment-proof')?.files[0];
                if (!proof) {
                    alert('Please upload proof of payment.');
                    return;
                }
                details.proof = proof.name;
                details.reference_code = generateReferenceCode();
            } else if (selectedDepositMethod === 'Card') {
                const cardNumber = document.getElementById('card-number')?.value;
                const cardExpiry = document.getElementById('card-expiry')?.value;
                const cardCvv = document.getElementById('card-cvv')?.value;
                if (!cardNumber || !cardExpiry || !cardCvv) {
                    alert('Please enter valid card details.');
                    return;
                }
                details.card_number = cardNumber;
                details.card_expiry = cardExpiry;
                details.card_cvv = cardCvv;
            } else if (selectedDepositMethod === 'Crypto') {
                const proof = document.getElementById('payment-proof')?.files[0];
                const coinType = document.getElementById('coin-type')?.value;
                if (!proof) {
                    alert('Please upload proof of payment.');
                    return;
                }
                details.proof = proof.name;
                details.coin_type = coinType;
                details.wallet_address = '0xAB123456789CDEFFED1234...';
            }

            const requests = loadRequests();
            const newRequest = {
                id: requests.length + 1,
                type: 'deposit',
                amount: amount,
                method: selectedDepositMethod,
                currency: depositCurrency.value,
                details: details,
                status: 'Pending',
                date: new Date().toLocaleDateString('en-US', { month: 'short', day: 'numeric', year: '2-digit' })
            };
            requests.push(newRequest);
            saveRequests(requests);

            alert(`Deposit request for $${amount.toFixed(2)} via ${selectedDepositMethod} submitted! Status: Pending`);
            depositAmount.value = '500';
            if (document.getElementById('payment-proof')) document.getElementById('payment-proof').value = '';
            if (document.getElementById('card-number')) document.getElementById('card-number').value = '';
            if (document.getElementById('card-expiry')) document.getElementById('card-expiry').value = '';
            if (document.getElementById('card-cvv')) document.getElementById('card-cvv').value = '';
            loadRecentTransactions();
        }

        // Submit withdrawal request
        function submitWithdrawRequest() {
            const amount = parseFloat(withdrawAmount.value);
            if (!amount || amount <= 0) {
                alert('Please enter a valid withdrawal amount.');
                return;
            }
            if (amount > 2450) {
                alert('Withdrawal amount exceeds available balance ($2,450.00).');
                return;
            }

            const destination = withdrawDestination.value;
            if (!destination) {
                alert('Please enter a valid destination.');
                return;
            }

            const security = withdrawSecurity.value;
            if (!security) {
                alert('Please enter OTP or PIN.');
                return;
            }

            let bankName = null;
            if (selectedWithdrawMethod === 'Bank Account') {
                bankName = bankNameSelect.value === 'Other' ? bankNameCustom.value : bankNameSelect.value;
                if (!bankName) {
                    alert('Please select or enter a valid bank name.');
                    return;
                }
            }

            const requests = loadRequests();
            const newRequest = {
                id: requests.length + 1,
                type: 'withdraw',
                amount: amount,
                method: selectedWithdrawMethod,
                destination: destination,
                bank_name: bankName,
                security: security,
                status: 'Pending',
                date: new Date().toLocaleDateString('en-US', { month: 'short', day: 'numeric', year: '2-digit' })
            };
            requests.push(newRequest);
            saveRequests(requests);

            alert(`Withdrawal request for $${amount.toFixed(2)} via ${selectedWithdrawMethod} submitted! Status: Pending`);
            withdrawAmount.value = '200';
            withdrawDestination.value = '';
            withdrawSecurity.value = '';
            bankNameSelect.value = 'GTBank';
            bankNameCustom.value = '';
            bankNameCustom.style.display = 'none';
            loadRecentTransactions();
        }

        // Submit managed account application
        function submitManagedRequest() {
            const capital = parseFloat(capitalSize.value);
            const profit = parseFloat(profitTarget.value);
            const lot = parseFloat(lotSize.value);
            const proofFile = proofExperience.files[0];
            const trackFile = trackRecord.files[0];

            if (!capital || capital < 1000) {
                alert('Please enter a valid capital size (minimum $1,000).');
                return;
            }
            if (!profit || profit < 0 || profit > 100) {
                alert('Please enter a valid profit target (0-100%).');
                return;
            }
            if (!lot || lot < 0.01) {
                alert('Please enter a valid lot size (minimum 0.01).');
                return;
            }
            if (!withdrawalTerms.value) {
                alert('Please describe withdrawal terms.');
                return;
            }
            if (!proofFile) {
                alert('Please upload proof of experience.');
                return;
            }
            if (!trackFile) {
                alert('Please upload track record.');
                return;
            }
            if (!tradingStyle.value) {
                alert('Please describe your trading style and bio.');
                return;
            }
            if (!termsAgreement.checked) {
                alert('You must agree to the terms and conditions.');
                return;
            }

            const requests = loadRequests();
            const newRequest = {
                id: requests.length + 1,
                type: 'managed-account',
                capital_size: capital,
                trader_type: traderType.value,
                return_cycle: returnCycle.value,
                profit_target: profit,
                withdrawal_terms: withdrawalTerms.value,
                lot_size: lot,
                experience_level: experienceLevel.value,
                proof_experience: proofFile.name,
                social_media: socialMedia.value || null,
                track_record: trackFile.name,
                trading_style: tradingStyle.value,
                risk_tolerance: riskTolerance.value || null,
                status: 'Pending',
                timestamp: new Date().toLocaleString('en-US')
            };
            requests.push(newRequest);
            saveRequests(requests);

            alert(`Managed Trading Account Application submitted! Status: Pending`);
            capitalSize.value = '';
            traderType.value = 'Forex';
            returnCycle.value = 'Daily';
            profitTarget.value = '';
            withdrawalTerms.value = '';
            lotSize.value = '';
            experienceLevel.value = 'Beginner';
            proofExperience.value = '';
            socialMedia.value = '';
            trackRecord.value = '';
            tradingStyle.value = '';
            riskTolerance.value = '';
            termsAgreement.checked = false;
            loadAllRequests();
        }

        // Load recent transactions
        function loadRecentTransactions() {
            const requests = loadRequests();
            recentDeposits.innerHTML = '';
            recentWithdrawals.innerHTML = '';

            const deposits = requests.filter(req => req.type === 'deposit').slice(-5);
            deposits.forEach(req => {
                const li = document.createElement('li');
                const method = req.method === 'Crypto' ? `Crypto (${req.details.coin_type || 'Unknown'})` : req.method;
                li.textContent = `${req.date} | ${method} | $${req.amount.toFixed(2)} | ${req.status === 'Completed' ? '✅ Completed' : '⏳ Pending'}`;
                recentDeposits.appendChild(li);
            });
            if (deposits.length === 0) recentDeposits.innerHTML = '<li>No recent deposits.</li>';

            const withdrawals = requests.filter(req => req.type === 'withdraw').slice(-5);
            withdrawals.forEach(req => {
                const li = document.createElement('li');
                const method = req.method === 'Bank Account' && req.bank_name ? `${req.method} (${req.bank_name})` : req.method;
                li.textContent = `${req.date} | ${method} | $${req.amount.toFixed(2)} | ${req.status === 'Completed' ? '✅ Completed' : '⏳ Pending'}`;
                recentWithdrawals.appendChild(li);
            });
            if (withdrawals.length === 0) recentWithdrawals.innerHTML = '<li>No recent withdrawals.</li>';
        }

        // View pending co-funding and managed account requests
        function viewPending() {
            const requests = loadRequests();
            const pending = requests.filter(req => (req.type === 'co-funding' || req.type === 'managed-account') && req.status === 'Pending');
            pendingList.innerHTML = '';

            if (pending.length === 0) {
                alert('No pending co-funding or managed account requests.');
                return;
            }

            pending.forEach(req => {
                const li = document.createElement('li');
                let text = `ID: ${req.id} | Type: ${req.type} | `;
                if (req.type === 'co-funding') {
                    text += `Firm: ${req.prop_firm} | Size: ${req.account_size} | Price: $${req.account_price.toFixed(2)} | Profit Split: ${req.profit_split} | Requester Share: $${req.requester_share.toFixed(2)} | Co-Funder Share: $${req.co_funder_share.toFixed(2)} | Status: ${req.status}`;
                    const acceptButton = document.createElement('button');
                    acceptButton.textContent = 'Accept as Co-Funder';
                    acceptButton.onclick = () => acceptRequest(req.id);
                    li.appendChild(acceptButton);
                } else if (req.type === 'managed-account') {
                    text += `Trader: ${req.trader_type} | Capital: $${req.capital_size.toFixed(2)} | Profit Target: ${req.profit_target}% | Return Cycle: ${req.return_cycle} | Status: ${req.status}`;
                    const acceptButton = document.createElement('button');
                    acceptButton.textContent = 'Accept Application';
                    acceptButton.onclick = () => acceptManagedRequest(req.id);
                    li.appendChild(acceptButton);
                }
                li.textContent = text;
                pendingList.appendChild(li);
            });

            pendingModal.style.display = 'flex';
        }

        // Accept co-funding request
        function acceptRequest(id) {
            const requests = loadRequests();
            const req = requests.find(r => r.id === id && r.type === 'co-funding' && r.status === 'Pending');
            if (!req) return;

            const total = req.requester_share + req.co_funder_share;
            if (Math.abs(total - req.account_price) > 0.01) {
                alert('Total shares do not match account price.');
                return;
            }

            req.status = 'Active';
            saveRequests(requests);
            alert(`Co-Funding Request ID ${id} accepted! Total funded: $${req.account_price.toFixed(2)}. Status: Active.`);
            pendingModal.style.display = 'none';
            loadAllRequests();
        }

        // Accept managed account request
        function acceptManagedRequest(id) {
            const requests = loadRequests();
            const req = requests.find(r => r.id === id && r.type === 'managed-account' && r.status === 'Pending');
            if (!req) return;

            req.status = 'Active';
            saveRequests(requests);
            alert(`Managed Account Application ID ${id} accepted! Status: Active.`);
            pendingModal.style.display = 'none';
            loadAllRequests();
        }

        // Load all requests (Investor/Admin Dashboard)
        function loadAllRequests() {
            const requests = loadRequests();
            allRequestsList.innerHTML = '';

            if (requests.length === 0) {
                allRequestsList.innerHTML = '<li>No requests available.</li>';
                return;
            }

            requests.forEach(req => {
                const li = document.createElement('li');
                let text = `ID: ${req.id} | Type: ${req.type} | `;
                if (req.type === 'co-funding') {
                    text += `Firm: ${req.prop_firm} | Size: ${req.account_size} | Price: $${req.account_price.toFixed(2)} | Profit Split: ${req.profit_split} | Requester Share: $${req.requester_share.toFixed(2)} | Co-Funder Share: $${req.co_funder_share.toFixed(2)}`;
                } else if (req.type === 'deposit') {
                    const method = req.method === 'Crypto' ? `Crypto (${req.details.coin_type || 'Unknown'})` : req.method;
                    text += `Amount: $${req.amount.toFixed(2)} | Method: ${method} | ${'proof' in req.details ? `Proof: ${req.details.proof}` : `Card: ${req.details.card_number}`}`;
                } else if (req.type === 'withdraw') {
                    const method = req.bank_name ? `${req.method} (${req.bank_name})` : req.method;
                    text += `Amount: $${req.amount.toFixed(2)} | Method: ${method} | Destination: ${req.destination}`;
                } else if (req.type === 'managed-account') {
                    text += `Trader: ${req.trader_type} | Capital: $${req.capital_size.toFixed(2)} | Profit Target: ${req.profit_target}% | Return Cycle: ${req.return_cycle} | Withdrawal Terms: ${req.withdrawal_terms} | Lot Size: ${req.lot_size} | Experience: ${req.experience_level}`;
                    if (req.risk_tolerance) text += ` | Risk Tolerance: ${req.risk_tolerance}`;
                    text += ` | Proof: ${req.proof_experience} (View) | Track Record: ${req.track_record} (View)`;
                    if (req.social_media) text += ` | Social: <a href="${req.social_media}" target="_blank">Profile</a>`;
                }
                text += ` | Status: ${req.status} | Timestamp: ${req.timestamp || req.date}`;
                li.innerHTML = text;

                if (req.status === 'Active') {
                    const completeButton = document.createElement('button');
                    completeButton.textContent = 'Mark as Completed';
                    completeButton.onclick = () => completeRequest(req.id);
                    li.appendChild(completeButton);
                }
                allRequestsList.appendChild(li);
            });
        }

        // Mark request as Completed
        function completeRequest(id) {
            const requests = loadRequests();
            const req = requests.find(r => r.id === id && r.status === 'Active');
            if (!req) return;

            req.status = 'Completed';
            saveRequests(requests);
            alert(`Request ID ${id} marked as Completed!`);
            loadAllRequests();
            loadRecentTransactions();
        }

        // Tab switching
        tabs.forEach(tab => {
            tab.addEventListener('click', () => {
                tabs.forEach(t => t.classList.remove('active'));
                tabContents.forEach(c => c.classList.remove('active'));
                tab.classList.add('active');
                document.getElementById(tab.dataset.tab).classList.add('active');
            });
        });

        // Deposit method switching
        depositMethodButtons.forEach(button => {
            button.addEventListener('click', () => {
                depositMethodButtons.forEach(btn => btn.classList.remove('active'));
                button.classList.add('active');
                selectedDepositMethod = button.dataset.method;
                updateDepositDetails();
            });
        });

        // Withdrawal method switching
        withdrawMethodButtons.forEach(button => {
            button.addEventListener('click', () => {
                withdrawMethodButtons.forEach(btn => btn.classList.remove('active'));
                button.classList.add('active');
                selectedWithdrawMethod = button.dataset.method;
                updateWithdrawDetails();
            });
        });

        // Bank name dropdown handling
        bankNameSelect.addEventListener('change', () => {
            bankNameCustom.style.display = bankNameSelect.value === 'Other' ? 'block' : 'none';
        });

        // Event listeners
        firmSelect.addEventListener('change', updateAccountSizes);
        profitSplitInput.addEventListener('input', calculateShares);
        submitButton.addEventListener('click', submitRequest);
        viewPendingButton.addEventListener('click', viewPending);
        closeModalButton.addEventListener('click', () => pendingModal.style.display = 'none');
        submitDeposit.addEventListener('click', submitDepositRequest);
        submitWithdraw.addEventListener('click', submitWithdrawRequest);
        submitManaged.addEventListener('click', submitManagedRequest);

        // Initial load
        updateAccountSizes();
        updateDepositDetails();
        updateWithdrawDetails();
        loadAllRequests();
        loadRecentTransactions();
    </script>
</body>
</html>
