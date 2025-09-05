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
        input, select { width: 100%; padding: 8px; margin-bottom: 10px; box-sizing: border-box; }
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
                    <button class="method-button active" data-method="Bank">💳 Bank</button>
                    <button class="method-button" data-method="Card">💵 Card</button>
                    <button class="method-button" data-method="Crypto">🔗 Crypto</button>
                </div>
                <label for="deposit-amount">Amount ($):</label>
                <input type="number" id="deposit-amount" min="1" value="500">
                <label for="deposit-currency">Currency:</label>
                <select id="deposit-currency">
                    <option value="USD">USD</option>
                </select>
                <div id="deposit-details">
                    <p><strong>Payment Details:</strong></p>
                    <p>Bank: Example Bank, Account: 1234-5678-9012, Routing: 987654321</p>
                    <label for="payment-proof">Upload Proof of Payment:</label>
                    <input type="file" id="payment-proof" accept="image/*,application/pdf">
                </div>
                <button id="submit-deposit">Confirm Deposit</button>
                <h2>Recent Deposits</h2>
                <ul id="recent-deposits" class="transaction-list"></ul>
            </div>
            <div id="withdraw-tab" class="tab-content">
                <h2>Withdraw Funds</h2>
                <p><strong>Available Balance:</strong> $2,450.00</p>
                <label for="withdraw-amount">Amount ($):</label>
                <input type="number" id="withdraw-amount" min="1" value="200">
                <label for="withdraw-method">Method:</label>
                <select id="withdraw-method">
                    <option value="Bank">🏦 Bank</option>
                    <option value="Crypto">🔗 Crypto</option>
                    <option value="Mobile Money">📲 Mobile Money</option>
                </select>
                <label for="withdraw-destination">Destination:</label>
                <input type="text" id="withdraw-destination" placeholder="Enter wallet address or bank account">
                <label for="withdraw-security">Security (OTP/PIN):</label>
                <input type="text" id="withdraw-security" placeholder="Enter OTP or PIN">
                <button id="submit-withdraw">Submit Withdrawal</button>
                <h2>Recent Withdrawals</h2>
                <ul id="recent-withdrawals" class="transaction-list"></ul>
            </div>
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
        const paymentProof = document.getElementById('payment-proof');
        const submitDeposit = document.getElementById('submit-deposit');
        const withdrawAmount = document.getElementById('withdraw-amount');
        const withdrawMethod = document.getElementById('withdraw-method');
        const withdrawDestination = document.getElementById('withdraw-destination');
        const withdrawSecurity = document.getElementById('withdraw-security');
        const submitWithdraw = document.getElementById('submit-withdraw');
        const depositDetails = document.getElementById('deposit-details');
        const recentDeposits = document.getElementById('recent-deposits');
        const recentWithdrawals = document.getElementById('recent-withdrawals');
        const methodButtons = document.querySelectorAll('.method-button');

        let selectedAccount = 'N/A';
        let accountPrice = 0.0;
        let selectedDepositMethod = 'Bank';

        // Load requests from localStorage
        function loadRequests() {
            return JSON.parse(localStorage.getItem('requests')) || [];
        }

        // Save requests to localStorage
        function saveRequests(requests) {
            localStorage.setItem('requests', JSON.stringify(requests));
        }

        // Update account sizes (Co-Funding)
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

        // Calculate shares (Co-Funding)
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
                date: new Date().toLocaleDateString('en-GB', { day: '2-digit', month: 'short' })
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
                    <p>Bank: Example Bank, Account: 1234-5678-9012, Routing: 987654321</p>
                    <label for="payment-proof">Upload Proof of Payment:</label>
                    <input type="file" id="payment-proof" accept="image/*,application/pdf">
                `,
                'Card': `
                    <p><strong>Payment Details:</strong></p>
                    <label for="card-number">Card Number:</label>
                    <input type="text" id="card-number" placeholder="1234-5678-9012-3456">
                `,
                'Crypto': `
                    <p><strong>Payment Details:</strong></p>
                    <p>Wallet Address: 0x1234567890abcdef</p>
                    <label for="payment-proof">Upload Proof of Payment:</label>
                    <input type="file" id="payment-proof" accept="image/*,application/pdf">
                `
            };
            depositDetails.innerHTML = details[selectedDepositMethod];
        }

        // Submit deposit request
        function submitDepositRequest() {
            const amount = parseFloat(depositAmount.value);
            if (!amount || amount <= 0) {
                alert('Please enter a valid deposit amount.');
                return;
            }

            let details = {};
            if (selectedDepositMethod === 'Bank' || selectedDepositMethod === 'Crypto') {
                const proof = document.getElementById('payment-proof')?.files[0];
                if (!proof) {
                    alert('Please upload proof of payment.');
                    return;
                }
                details.proof = proof.name;
            } else if (selectedDepositMethod === 'Card') {
                const cardNumber = document.getElementById('card-number')?.value;
                if (!cardNumber) {
                    alert('Please enter a valid card number.');
                    return;
                }
                details.card_number = cardNumber;
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
                date: new Date().toLocaleDateString('en-GB', { day: '2-digit', month: 'short' })
            };
            requests.push(newRequest);
            saveRequests(requests);

            alert(`Deposit request for $${amount.toFixed(2)} via ${selectedDepositMethod} submitted! Status: Pending`);
            depositAmount.value = '500';
            if (document.getElementById('payment-proof')) document.getElementById('payment-proof').value = '';
            if (document.getElementById('card-number')) document.getElementById('card-number').value = '';
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

            const method = withdrawMethod.value;
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

            const requests = loadRequests();
            const newRequest = {
                id: requests.length + 1,
                type: 'withdraw',
                amount: amount,
                method: method,
                destination: destination,
                security: security,
                status: 'Pending',
                date: new Date().toLocaleDateString('en-GB', { day: '2-digit', month: 'short' })
            };
            requests.push(newRequest);
            saveRequests(requests);

            alert(`Withdrawal request for $${amount.toFixed(2)} via ${method} submitted! Status: Pending`);
            withdrawAmount.value = '200';
            withdrawMethod.value = 'Bank';
            withdrawDestination.value = '';
            withdrawSecurity.value = '';
            loadRecentTransactions();
        }

        // Load recent transactions
        function loadRecentTransactions() {
            const requests = loadRequests();
            recentDeposits.innerHTML = '';
            recentWithdrawals.innerHTML = '';

            const deposits = requests.filter(req => req.type === 'deposit').slice(-5);
            deposits.forEach(req => {
                const li = document.createElement('li');
                li.textContent = `${req.date} | ${req.method} | $${req.amount.toFixed(2)} | ${req.status === 'Completed' ? '✅ Completed' : '⏳ Pending'}`;
                recentDeposits.appendChild(li);
            });
            if (deposits.length === 0) recentDeposits.innerHTML = '<li>No recent deposits.</li>';

            const withdrawals = requests.filter(req => req.type === 'withdraw').slice(-5);
            withdrawals.forEach(req => {
                const li = document.createElement('li');
                li.textContent = `${req.date} | ${req.method} | $${req.amount.toFixed(2)} | ${req.status === 'Completed' ? '✅ Completed' : '⏳ Pending'}`;
                recentWithdrawals.appendChild(li);
            });
            if (withdrawals.length === 0) recentWithdrawals.innerHTML = '<li>No recent withdrawals.</li>';
        }

        // View pending co-funding requests
        function viewPending() {
            const requests = loadRequests();
            const pending = requests.filter(req => req.type === 'co-funding' && req.status === 'Pending');
            pendingList.innerHTML = '';

            if (pending.length === 0) {
                alert('No pending co-funding requests.');
                return;
            }

            pending.forEach(req => {
                const li = document.createElement('li');
                li.textContent = `ID: ${req.id} | Firm: ${req.prop_firm} | Size: ${req.account_size} | Price: $${req.account_price.toFixed(2)} | Profit Split: ${req.profit_split} | Requester Share: $${req.requester_share.toFixed(2)} (Escrow) | Co-Funder Share: $${req.co_funder_share.toFixed(2)} | Status: ${req.status}`;
                const acceptButton = document.createElement('button');
                acceptButton.textContent = 'Accept as Co-Funder';
                acceptButton.onclick = () => acceptRequest(req.id);
                li.appendChild(acceptButton);
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
            alert(`Request ID ${id} accepted! Total funded: $${req.account_price.toFixed(2)}. Status: Active. Account ready for purchase.`);
            pendingModal.style.display = 'none';
            loadAllRequests();
        }

        // Load all requests
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
                    text += `Amount: $${req.amount.toFixed(2)} | Method: ${req.method} | ${'proof' in req.details ? `Proof: ${req.details.proof}` : `Card: ${req.details.card_number}`}`;
                } else if (req.type === 'withdraw') {
                    text += `Amount: $${req.amount.toFixed(2)} | Method: ${req.method} | Destination: ${req.destination}`;
                }
                text += ` | Status: ${req.status}`;
                li.textContent = text;

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
        methodButtons.forEach(button => {
            button.addEventListener('click', () => {
                methodButtons.forEach(btn => btn.classList.remove('active'));
                button.classList.add('active');
                selectedDepositMethod = button.dataset.method;
                updateDepositDetails();
            });
        });

        // Event listeners
        firmSelect.addEventListener('change', updateAccountSizes);
        profitSplitInput.addEventListener('input', calculateShares);
        submitButton.addEventListener('click', submitRequest);
        viewPendingButton.addEventListener('click', viewPending);
        closeModalButton.addEventListener('click', () => pendingModal.style.display = 'none');
        submitDeposit.addEventListener('click', submitDepositRequest);
        submitWithdraw.addEventListener('click', submitWithdrawRequest);

        // Initial load
        updateAccountSizes();
        updateDepositDetails();
        loadAllRequests();
        loadRecentTransactions();
    </script>
</body>
</html>
