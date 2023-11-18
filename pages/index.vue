<template>
    <div class="container">
		<nav role="navigation" class="wheader">
			<div id="menuToggle">
				<input type="checkbox" />
				<span></span>
				<span></span>
				<span></span>
				<ul id="menu">
					<li><a class="social-link" href="https://lyncoin.com" title="Lyncoin" target="_blank" rel="nofollow noopener">Website</a></li>
					<li><a class="social-link" href="https://lcnxp.com" title="Lyncoin Explorer" target="_blank" rel="nofollow noopener">Explorer</a></li>
					<li><a class="social-link" href="https://github.com/lyncoin/lyncoin-webwallet/" title="Source Code" target="_blank" rel="nofollow noopener">Source Code</a></li>
					<li><a class="social-link" @click="exit">Exit</a></li>
				</ul>
			</div>
			<span class="wallettitle">Lyncoin Web Wallet (v{{$nuxt.$config.clientVersion}})</span>
		</nav>
		<div class="wcontent">
			<div class="page" id="page-setpassword">
				<p class="title is-3 is-spaced">Set password</p>
				<div class="field">
				  <p class="control has-icons-left">
					<input name="wallet-password1" class="input" autocomplete="off" readonly onfocus="this.removeAttribute('readonly');" type="password" placeholder="Password">
					<span class="icon is-small is-left">
					  <i class="fas fa-lock"></i>
					</span>
				  </p>
				</div>
				<div class="field">
				  <p class="control has-icons-left">
					<input name="wallet-password2" class="input" autocomplete="off" readonly onfocus="this.removeAttribute('readonly');" type="password" placeholder="Confirm Password">
					<span class="icon is-small is-left">
					  <i class="fas fa-lock"></i>
					</span>
				  </p>
				</div>
				<div class="field">
				  <p class="control">
					<button class="button is-success" @click="set_password">
					  Set Password
					</button>
				  </p>
				</div>
			</div>

			<div class="page" id="page-choose">
				<div class="columns">
				  <div class="column">
					<div class="control">
						<button class="button is-primary" style="width: 100%;" @click="generate_wallet">Generate Wallet</button>
					</div>
				  </div>
				  <div class="column">
					<div class="control">
						<button class="button is-primary" style="width: 100%;" @click="go_to_recovery_wallet">Recover wallet</button>
					</div>
				  </div>
				</div>
			</div>

			<div class="page" id="show-recovery">
				<article class="message is-info">
				  <div class="message-header">
					<p>Your recovery key:</p>
				  </div>
				  <div class="message-body">
					<!--p oncopy="return false" oncut="return false" onpaste="return false"></p-->
					<p></p>
					<button class="button is-primary is-rounded" @click="generate_wallet">Regenerate</button>
					<button class="button is-primary is-rounded" @click="go_to_recovery_confirm">I wrote it down</button>
				  </div>
				</article>
			</div>

			<div class="page" id="recover-wallet">
				<article class="message is-info">
				  <div class="message-header">
					<p>Enter your recovery key:</p>
				  </div>
				  <div class="message-body">
					<textarea class="textarea" placeholder=""></textarea>
					<p>Enter number of addresses</p>
					<input class="input is-rounded" type="text" placeholder="1">
					<button class="button is-primary is-rounded" style="margin-top: 10px;" @click="recovery_wallet">OK</button>
				  </div>
				</article>
			</div>

			<div class="page" id="recovery-confirm">
				<article class="message is-info">
				  <div class="message-header">
					<p>Confirm your recovery key:</p>
				  </div>
				  <div class="message-body">
					<textarea class="textarea" placeholder=""></textarea>
					<button class="button is-primary is-rounded" style="margin-top: 10px;" @click="confirm_recovery_key">OK</button>
				  </div>
				</article>
			</div>

			<div class="page" id="page-receive">
				<button class="button is-primary" style="margin-top: 10px;" @click="create_address">Create a receiving address</button>
				<div id="addrlist"></div>
			</div>

			<div class="page" id="page-show-recovery-key">
				<div class="wdialog" id="show-recovery-key-form">
					<p style="word-wrap: break-word;"></p>
				</div>
			</div>

			<div class="page" id="page-ask-password-show-recovery-key">
				<div class="wdialog" id="ask-password-show-recovery-key-form">
					<div class="field">
					  <p class="control has-icons-left">
						<input class="input" autocomplete="off" readonly onfocus="this.removeAttribute('readonly');" type="password" placeholder="Password">
						<span class="icon is-small is-left">
						  <i class="fas fa-lock"></i>
						</span>
					  </p>
					</div>
					<div class="field">
					  <p class="control">
						<button class="button is-success" @click="show_recovery_key_real">
						  Ok
						</button>
					  </p>
					</div>
				</div>
			</div>

			<div class="page" id="page-show-privatekey">
				<div class="wdialog" id="show-privatekey-form">
					<p style="word-wrap: break-word;"></p>
				</div>
			</div>

			<div class="page" id="page-ask-password-show-privatekey">
				<div class="wdialog" id="ask-password-show-privatekey-form">
					<div class="field">
					  <p class="control has-icons-left">
						<input class="input" autocomplete="off" readonly onfocus="this.removeAttribute('readonly');" type="password" placeholder="Password">
						<span class="icon is-small is-left">
						  <i class="fas fa-lock"></i>
						</span>
					  </p>
					</div>
					<div class="field">
					  <p class="control">
						<button class="button is-success" @click="show_privatekey_real">
						  Ok
						</button>
					  </p>
					</div>
				</div>
			</div>

			<div class="page" id="page-ask-password-create-address">
				<div class="wdialog" id="ask-password-create-address-form">
					<div class="field">
					  <p class="control has-icons-left">
						<input class="input" autocomplete="off" readonly onfocus="this.removeAttribute('readonly');" type="password" placeholder="Password">
						<span class="icon is-small is-left">
						  <i class="fas fa-lock"></i>
						</span>
					  </p>
					</div>
					<div class="field">
					  <p class="control">
						<button class="button is-success" @click="create_address_real">
						  Ok
						</button>
					  </p>
					</div>
				</div>
			</div>

			<div class="page" id="page-ask-password-send">
				<div class="wdialog" id="ask-password-send-form">
					<div class="field">
					  <p class="control has-icons-left">
						<input class="input" autocomplete="off" readonly onfocus="this.removeAttribute('readonly');" type="password" placeholder="Password">
						<span class="icon is-small is-left">
						  <i class="fas fa-lock"></i>
						</span>
					  </p>
					</div>
					<div class="field">
					  <p class="control">
						<button class="button is-success" @click="sendtx_real">
						  Ok
						</button>
					  </p>
					</div>
				</div>
			</div>

			<div class="page" id="page-send">
				<p class="title is-3 is-spaced">Send</p>
				<p class="subtitle is-5">Recipient</p>
				<input id="page_send_address" class="input is-rounded" type="text" placeholder="lc1...">
				<p class="subtitle is-5">Amount</p>
				<input id="page_send_amount" class="input is-rounded" type="text" placeholder="0.00">
				<button class="button is-primary" style="margin-top: 10px;" @click="sendtx">Send</button>
			</div>

			<div class="page" id="page-home">
				<div class="card">
				  <div class="card-content">
					<div class="content">
					      <div class="home-item">Balance: <span id="balance">N/A</span> {{symbol}}</div>
					      <div class="home-item">Pending: <span id="pending">N/A</span> {{symbol}}</div>
					      <div class="home-item">Unconfirmed: <span id="unconfirmed">N/a</span> {{symbol}}</div>
					</div>
				  </div>
				</div>
			</div>

			<div class="page" id="page-transactions">

			</div>

			<div class="page" id="page-settings">
				<button class="button is-primary" style="width: 100%;" @click="show_recovery_key">Show Recovery Key</button>
			</div>

		</div>
		<nav class="wfooter" role="menulist">
			<a data-page="page-receive" href="#!" role="menuitem">
				<i aria-hidden="true" class="fa-solid fa-download"></i> Receive
			</a>
			<a data-page="page-send" href="#!" role="menuitem">
				<i aria-hidden="true" class="fa-solid fa-share"></i> Send
			</a>
			<a data-page="page-home" href="#!" role="menuitem" class="active" aria-describedby="nav-current">
				<i aria-hidden="true" class="fa-solid fa-house"></i> Home
			</a>
			<a data-page="page-transactions" href="#!" role="menuitem">
				<i aria-hidden="true" class="fa-solid fa-money-bill-transfer"></i> Transactions
			</a>
			<a data-page="page-settings" href="#!" role="menuitem">
				<i aria-hidden="true" class="fa-solid fa-gear"></i> Settings
			</a>
			<i class="line" id="nav-current">current item</i>
		</nav>
    </div>
</template>

<script>
export default {
  name: 'IndexPage',
  data() {
    return {
      symbol: '',
      title: 'Lyncoin Web Wallet',
    }
  },
  fetchOnServer: false,
  async fetch() {
    this.preload();
    this.load();
    this.get_balance();
    this.load_transactions();
    this.updater();
  },
  methods: {
	updater() {
		if (!webwallet.updater_is_loading) {
			webwallet.updater_is_loading = true;
			$nuxt.$axios({
				method: 'GET',
				url: webwallet.explorer_api_url + '/webwallet_version',
				withCredentials: false,
				crossDomain: true,
				headers: {
					'Content-Type': 'application/json; charset=utf-8',
				},
			}).then(function (res) {
				if(res.data.result !== $nuxt.$config.clientVersion) {
					location.reload(true);
				}
			}).catch(function (error) {
				alertifyjs.error(error + "");
			}).finally(function () {
				webwallet.updater_is_loading = false;
			});
		}
		setTimeout(this.updater, 10000);
	},
	exit() {
		$('.page').css('display', 'none');
		$('.wfooter').css('display', 'none');
		$('#page-setpassword').css('display', 'block');
		$('#menuToggle input[type="checkbox"]').prop( "checked", false );
		localStorage.clear();
		$('#addrlist').empty();
		setTimeout(function() {
			$('#balance').text('N/A');
			$('#pending').text('N/A');
			$('#unconfirmed').text('N/A');
		}, 3000);
	},
	recovery_wallet() {
		var key = $('#recover-wallet textarea').val();
		var address_count = $('#recover-wallet input').val();
		var invalid_amount = false;
		address_count = parseInt(address_count);
		if(isNaN(address_count)) {
			invalid_amount = true;
		} else if (address_count < 1) {
			invalid_amount = true;
		} else if (address_count > 100) {
			invalid_amount = true;
		}
		if (invalid_amount) {
			alertifyjs.error("Invalid number of addresses");
		} else {
			if(!webwallet.bip39.validateMnemonic(key)) {
				alertifyjs.error("Invalid recovery key");
			} else {
				const seed = webwallet.bip39.mnemonicToSeedSync(key);
				let root = webwallet.bip32.fromSeed(seed, webwallet.network);
				let account = root.derivePath(webwallet.bip44_path);
				for(var i=0;i < address_count; i++) {
					let node = account.derive(0).derive(i);
					let p2wpkh = webwallet.bitcoin.payments.p2wpkh({
						pubkey: node.publicKey,
						network: webwallet.network,
					});
					let addr = p2wpkh.address;
					localStorage.setItem('addr' + i, addr);
					var key2 = webwallet.encrypt(webwallet.tmp, node.toWIF());
					localStorage.setItem('addrkey' + i, key2);

					var tcard = $('<div/>');
					tcard.addClass('card');
					$('#addrlist').append(tcard);
					
					var tcardcontent = $('<div/>');
					tcardcontent.addClass('card-content');
					tcard.append(tcardcontent);

					var tcontent = $('<div/>');
					tcontent.addClass('content');
					tcardcontent.append(tcontent);
					
					var tcolumns = $('<div/>');
					tcolumns.addClass('columns is-flex-direction-row is-flex-wrap-wrap');
					tcontent.append(tcolumns);
					
					var tcol = $('<div/>');
					tcol.addClass('column is-two-thirds');
					tcol.text(addr);
					
					tcolumns.append(tcol);

					tcol = $('<div/>');
					tcol.addClass('column is-one-third');

					var tbtn = $('<button/>');
					tbtn.addClass('button is-primary is-rounded');
					tbtn.data('index', i);
					tbtn.text('Show Private Key');
					tbtn.click(webwallet.show_privatekey);
					tcol.append(tbtn);
					
					tcolumns.append(tcol);
				}
				localStorage.setItem('address_count', address_count);
				var encrypted = webwallet.encrypt(webwallet.tmp, key);
				localStorage.setItem('key', encrypted);
				webwallet.tmp = '';
				$('.page').css('display', 'none');
				$('.wfooter').css('display', 'block');
				$('[data-page="page-home"]')[0].click();
			}
		}
	},
	go_to_recovery_wallet() {
		$('#page-choose').css('display', 'none');
		$('#recover-wallet').css('display', 'block');
	},
	show_recovery_key_real() {
		$('.ajs-close').click();
		var pas = $('#ask-password-show-recovery-key-form input[type="password"]').val();
		$('#ask-password-show-recovery-key-form input[type="password"]').val('');
		var key = localStorage.getItem('key');
		try {
			var decrypted = webwallet.decrypt(pas, key);
			$('#show-recovery-key-form p').text(decrypted);
			alertifyjs.send_dialog($('#show-recovery-key-form')[0]).set({onclose:function(){$('#show-recovery-key-form p').text('');}}); 
		} catch (error) {
			alertifyjs.error('Invalid password');
		}
	},
	show_recovery_key() {
		alertifyjs.recovery_key_dialog($('#ask-password-show-recovery-key-form')[0]);
	},
	show_privatekey_real() {
		$('.ajs-close').click();
		var pas = $('#ask-password-show-privatekey-form input[type="password"]').val();
		$('#ask-password-show-privatekey-form input[type="password"]').val('');
		var key = localStorage.getItem('addrkey' + webwallet.keyindex);
		try {
			var decrypted = webwallet.decrypt(pas, key);
			$('#show-privatekey-form p').text(decrypted);
			alertifyjs.send_dialog($('#show-privatekey-form')[0]).set({onclose:function(){$('#show-privatekey-form p').text('');}}); 
		} catch (error) {
			alertifyjs.error('Invalid password');
		}
	},
	sendtx_real() {
		$('.ajs-close').click();
		var pas = $('#ask-password-send-form input[type="password"]').val();
		$('#ask-password-send-form input[type="password"]').val('');
		var key = localStorage.getItem('key');
		try {
			var decrypted = webwallet.decrypt(pas, key);
		} catch (error) {
			alertifyjs.error('Invalid password');
		} finally {
			webwallet.tmp = pas;
			if (!webwallet.sending) {
				webwallet.sending = true;
				var address_count = localStorage.getItem('address_count');
				if(address_count === null) {
					address_count = 0;
				} else {
					address_count = parseInt(address_count);
				}
				var taddr;
				var addr_item;
				var i;
				webwallet.utxo_addresses = [];
				for(i=0; i<address_count; i++) {
					var taddr = localStorage.getItem('addr' + i);
					webwallet.utxo_addresses.push(taddr);
				}
				$nuxt.$axios({
					async: false,
					method: 'POST',
					url: webwallet.explorer_api_url + '/unspentinputs',
					withCredentials: false,
					crossDomain: true,
					headers: {
						'Content-Type': 'application/json; charset=utf-8',
					},
					data: {
						addresses: webwallet.utxo_addresses,
						value: webwallet.amount,
						fee: 0,
					}
				}).then(function (res) {
					if(res.data.result.value < webwallet.amount) {
						alertifyjs.error('Insufficient balance');
					} else {
						var tx = webwallet.create_tx(res, webwallet.to_addr, pas);
						var vsize = tx.virtualSize();
						webwallet.fee = webwallet.feerate * (vsize/1024);
						webwallet.fee = webwallet.fee.toFixed(webwallet.decimals);
						$nuxt.$axios({
							async: false,
							method: 'POST',
							url: webwallet.explorer_api_url + '/unspentinputs',
							withCredentials: false,
							crossDomain: true,
							headers: {
								'Content-Type': 'application/json; charset=utf-8',
							},
							data: {
								addresses: webwallet.utxo_addresses,
								value: webwallet.amount,
								fee: webwallet.fee,
							}
						}).then(function (res) {
							if(res.data.result.value < webwallet.amount) {
								alertifyjs.error('Insufficient balance');
							} else {
								var tx = webwallet.create_tx(res, webwallet.to_addr, pas);
								var vsize = tx.virtualSize();
								var fee = webwallet.feerate * (vsize/1024);
								fee = fee.toFixed(webwallet.decimals);
								webwallet.rawtx = tx.toHex();
								$nuxt.$axios({
									method: 'POST',
									url: webwallet.explorer_api_url + '/pushtx',
									withCredentials: false,
									crossDomain: true,
									headers: {
										'Content-Type': 'application/json; charset=utf-8',
									},
									data: {
										tx: webwallet.rawtx,
									}
								}).then(function (res) {
									if(res.data.code == 0) {
										alertifyjs.success('Transaction was successfully sent')
									} else {
										alertifyjs.error(res.data.error);
									}
								}).catch(function (error) {
									alertifyjs.error(error + "");
								}).finally(function () {});
							}
						}).catch(function (error) {
							alertifyjs.error(error + "");
						});
					}
				}).catch(function (error) {
					alertifyjs.error(error + "");
				});
				webwallet.sending = false;
			}
		}
	},
	create_address_real() {
		$('.ajs-close').click();
		var pas = $('#ask-password-create-address-form input[type="password"]').val();
		$('#ask-password-create-address-form input[type="password"]').val('');
		var key = localStorage.getItem('key');
		try {
			var decrypted = webwallet.decrypt(pas, key);
		} catch (error) {
			alertifyjs.error('Invalid password');
		} finally {
			var address_count = localStorage.getItem('address_count');
			if(address_count === null) {
				address_count = 0;
			} else {
				address_count = parseInt(address_count);
			}
			const seed = webwallet.bip39.mnemonicToSeedSync(decrypted);
			let root = webwallet.bip32.fromSeed(seed, webwallet.network);
			let account = root.derivePath(webwallet.bip44_path);
			let node = account.derive(0).derive(address_count);
			let p2wpkh = webwallet.bitcoin.payments.p2wpkh({
				pubkey: node.publicKey,
				network: webwallet.network,
			});
			let addr = p2wpkh.address;
			localStorage.setItem('addr' + address_count, addr);
			var key2 = webwallet.encrypt(pas, node.toWIF());
			localStorage.setItem('addrkey' + address_count, key2);
			localStorage.setItem('address_count', address_count + 1);

			var tcard = $('<div/>');
			tcard.addClass('card');
			$('#addrlist').append(tcard);
			
			var tcardcontent = $('<div/>');
			tcardcontent.addClass('card-content');
			tcard.append(tcardcontent);

			var tcontent = $('<div/>');
			tcontent.addClass('content');
			tcardcontent.append(tcontent);
			
			var tcolumns = $('<div/>');
			tcolumns.addClass('columns is-flex-direction-row is-flex-wrap-wrap');
			tcontent.append(tcolumns);
			
			var tcol = $('<div/>');
			tcol.addClass('column is-two-thirds');
			tcol.text(addr);
			
			tcolumns.append(tcol);

			tcol = $('<div/>');
			tcol.addClass('column is-one-third');

			var tbtn = $('<button/>');
			tbtn.addClass('button is-primary is-rounded');
			tbtn.data('index', address_count);
			tbtn.text('Show Private Key');
			tbtn.click(webwallet.show_privatekey);
			tcol.append(tbtn);
			
			tcolumns.append(tcol);

		}
	},
	sendtx() {

		webwallet.to_addr = $('#page_send_address').val();
		webwallet.amount = $('#page_send_amount').val();
		var parts = webwallet.amount.split(".");

		var invalid_amount = false;
		if(isNaN(parseFloat(webwallet.amount))) {
			invalid_amount = true;
		} else if(parseFloat(webwallet.amount) < 0 || parseFloat(webwallet.amount) > webwallet.max_supply) {
			invalid_amount = true;
		} else if(parts.length == 2 && parts[1].length > webwallet.decimals) {
			invalid_amount = true;
		} else {
			webwallet.amount = parseFloat(webwallet.amount);
		}

		var invalid_addr = false;
		try {
			webwallet.to_addr = webwallet.bitcoin.address.toOutputScript(webwallet.to_addr, webwallet.network);
		} catch {
			invalid_addr = true;
		} finally {
		
		}
		if (invalid_amount) {
			alertifyjs.error('Invalid amount');
		}else if (invalid_addr) {
			alertifyjs.error('Invalid address');
		} else {
			alertifyjs.send_dialog($('#ask-password-send-form')[0]);
		}
	},
	load_transactions() {
		if (localStorage.getItem('key') !== null) {
			if (!webwallet.transactions_are_loading) {
				webwallet.transactions_are_loading = true;
				var address_count = localStorage.getItem('address_count');
				if(address_count === null) {
					address_count = 0;
				} else {
					address_count = parseInt(address_count);
				}
				var taddr;
				var addr_item;
				var i;
				webwallet.txs_addresses = [];
				for(i=0; i<address_count; i++) {
					var taddr = localStorage.getItem('addr' + i);
					webwallet.txs_addresses.push(taddr);
				}
				$nuxt.$axios({
					method: 'POST',
					url: webwallet.explorer_api_url + '/txs',
					withCredentials: false,
					crossDomain: true,
					headers: {
						'Content-Type': 'application/json; charset=utf-8',
					},
					data: {
						addresses: webwallet.txs_addresses,
					}
				}).then(function (res) {
					$('#page-transactions').empty();
					for(var i = 0; i< res.data.result.length; i++) {
						var tx = res.data.result[i];
						var txid = tx.txid;
						
						var is_send = true;
						var outval = 0;
						var inval = 0;

						for(var j = 0; j< tx.inputs.length; j++) {
							var input = tx.inputs[j];
							if(webwallet.txs_addresses.indexOf(input.address) === -1) {
								is_send = false;
							}
						}
						for(var j = 0; j< tx.outputs.length; j++) {
							var output = tx.outputs[j];
							if(webwallet.txs_addresses.indexOf(output.address) === -1) {
								outval += parseFloat(output.value);
							} else {
								inval += parseFloat(output.value);
							}
						}
						//if(is_send) {
						//	console.log(txid + ' Sent: ' + outval);
						//} else {
						//	console.log(txid + ' Got: ' + inval);
						//}

						var card = $('<div/>');
						card.addClass('card');
						card.addClass('tx');
						$('#page-transactions').append(card);

						if(is_send) {
							card.addClass('senttx');
						} else {
							card.addClass('receivedtx');
						}

						var card_content = $('<div/>');
						card_content.addClass('card-content');
						card.append(card_content);

						var content = $('<div/>');
						content.addClass('content');
						card_content.append(content);

						var linkitem = $('<div/>');
						linkitem.addClass('linkitem');
						content.append(linkitem);

						var linktext = $('<span/>');
						linktext.text('TxID: ');
						linkitem.append(linktext);

						var link = $('<a/>');
						link.attr('href', webwallet.explorer_url + '/tx/' + txid);
						link.attr('target', '_blank');
						link.text(txid);
						linkitem.append(link);

					}
				}).catch(function (error) {
					alertifyjs.error(error + "");
				}).finally(function () {
					webwallet.transactions_are_loading = false;
				});
			}
		}
		setTimeout(this.load_transactions, 3000);
	},
	get_balance() {
		if (localStorage.getItem('key') !== null) {
			if (!webwallet.balance_is_loading) {
				webwallet.balance_is_loading = true;
				var address_count = localStorage.getItem('address_count');
				if(address_count === null) {
					address_count = 0;
				} else {
					address_count = parseInt(address_count);
				}
				var taddr;
				var addr_item;
				var i;
				webwallet.balance_addresses = [];
				for(i=0; i<address_count; i++) {
					var taddr = localStorage.getItem('addr' + i);
					webwallet.balance_addresses.push(taddr);
				}
				$nuxt.$axios({
					method: 'POST',
					url: webwallet.explorer_api_url + '/address',
					withCredentials: false,
					crossDomain: true,
					headers: {
						'Content-Type': 'application/json; charset=utf-8',
					},
					data: {
						addresses: webwallet.balance_addresses,
					}
				}).then(function (res) {
					$('#balance').text(res.data.result.balance);
					$('#pending').text(res.data.result.immature_balance);
					$('#unconfirmed').text(res.data.result.unconfirmed_balance);
				}).catch(function (error) {
					alertifyjs.error(error + "");
				}).finally(function () {
					webwallet.balance_is_loading = false;
				});
			}
		}
		setTimeout(this.get_balance, 3000);
	},
	create_address() {
		var address_count = localStorage.getItem('address_count');
		address_count = parseInt(address_count);
		if(address_count>=100) {
			alertifyjs.error('Cannot create more than 100 addresses');
		} else {
			alertifyjs.create_address_dialog($('#ask-password-create-address-form')[0]);
		}
	},
	generate_wallet() {
		let mnemonic = webwallet.bip39.generateMnemonic();
		webwallet.tmp2 = mnemonic;
		$('#show-recovery .message-body p').text(mnemonic);
		$('#page-choose').css('display', 'none');
		$('#show-recovery').css('display', 'block');
	},
	go_to_recovery_confirm() {
		$('#show-recovery').css('display', 'none');
		$('#recovery-confirm').css('display', 'block');
	},
	confirm_recovery_key() {
		var key = $('#recovery-confirm textarea').val();
		if(key !== webwallet.tmp2) {
			alertifyjs.error("Invalid recovery key");
		} else {
			var encrypted = webwallet.encrypt(webwallet.tmp, webwallet.tmp2);
			localStorage.setItem('key', encrypted);
			$('#recovery-confirm').css('display', 'none');
			$('.wfooter').css('display', 'block');
			$('[data-page="page-home"]')[0].click();
			webwallet.tmp = '';
			webwallet.tmp2 = '';
		}
	},
	set_password() {
		var pas1 = $('[name=wallet-password1]').val();
		var pas2 = $('[name=wallet-password2]').val();
		if(pas1.length < 4) {
			alertifyjs.error("Password is too short");
		} else {
			if(pas1!==pas2) {
				alertifyjs.error("Password doesn't match");
			} else {
				webwallet.tmp = pas1;
				$('[name=wallet-password1]').val('');
				$('[name=wallet-password2]').val('');
				$('#page-setpassword').css('display', 'none');
				$('#page-choose').css('display', 'block');
			}
		}
	},
	preload() {
		window.webwallet = {};

		window.alertifyjs = require('alertifyjs');
		window.$ = require('jquery');
		window.webwallet.bip32lib = require('bip32');
		webwallet.ecc = require('@bitcoinerlab/secp256k1');
		webwallet.ecpairlib = require('ecpair');
		webwallet.bip32 = webwallet.bip32lib.BIP32Factory(webwallet.ecc);
		webwallet.ecpair = webwallet.ecpairlib.ECPairFactory(webwallet.ecc);
		webwallet.bip39 = require('bip39');
		webwallet.bitcoin = require('bitcoinjs-lib');
		webwallet.crypto = require('crypto');

		webwallet.network = {
		  messagePrefix: '\x18Lyncoin Signed Message:\n',
		  bech32: 'lc',
		  bip32: {
			public: 0x019c354f,
			private: 0x019c3115,
		  },
		  pubKeyHash: 0xea,
		  scriptHash: 0x37,
		  wif: 0x7e,
		};
		
		webwallet.feerate = 0.0001;
		webwallet.decimals = 8;
		webwallet.max_supply = 91000000000;
		this.symbol = 'LCN';
		webwallet.explorer_url = 'https://lcnxp.com';
		//webwallet.explorer_api_url = 'http://127.0.0.1:3517';
		webwallet.explorer_api_url = 'https://api.lcnxp.com';

		webwallet.bip44_path = `m/44'/5813'/0'/0`;
		
		webwallet.encryption_algo = 'aes-256-cbc';

		webwallet.encrypt = function(key, x) {
			var keyhash = webwallet.crypto.createHash('sha256');
			keyhash.write(key);
			keyhash = keyhash.digest();
			var algorithm = webwallet.encryption_algo;
			var iv = webwallet.crypto.randomBytes(16);
			var cipher = webwallet.crypto.createCipheriv(algorithm, keyhash, iv);
			var encrypted = cipher.update(x);
			encrypted = Buffer.concat([encrypted, cipher.final()]);
			return iv.toString('hex') + ":" + encrypted.toString('hex');
		};

		webwallet.decrypt = function(key, x) {
			var keyhash = webwallet.crypto.createHash('sha256');
			keyhash.write(key);
			keyhash = keyhash.digest();
			var b = x.split(":");
			var iv = Buffer.from(b[0], 'hex');
			var encrypted = Buffer.from(b[1], 'hex');
			var decipher = webwallet.crypto.createDecipheriv(webwallet.encryption_algo, keyhash, iv);
			var decrypted = decipher.update(encrypted);
			decrypted = Buffer.concat([decrypted, decipher.final()]);
			return decrypted.toString();
		};
		
		webwallet.create_tx = function(res, to_addr, pas) {
			var i;
			const psbt = new webwallet.bitcoin.Psbt({ network: webwallet.network });
			for(i = 0; i<res.data.result.inputs.length; i++) {
				var addr = localStorage.getItem('addr' + res.data.result.inputs[i][2]);
				psbt.addInput({
				  hash: res.data.result.inputs[i][0],
				  index: res.data.result.inputs[i][1],
				  witnessUtxo: {
					script: webwallet.bitcoin.address.toOutputScript(addr, webwallet.network),
					value: res.data.result.inputs[i][3]
				  },
				});
			}
			psbt.addOutput({
			  script: to_addr,
			  value: res.data.result.send
			});
			if(res.data.result.return>0) {
				var return_addr = webwallet.bitcoin.address.toOutputScript(localStorage.getItem('addr0'), webwallet.network);
				psbt.addOutput({
				  script: return_addr,
				  value: res.data.result.return
				});
			}
			var address_count = localStorage.getItem('address_count');
			if(address_count === null) {
				address_count = 0;
			} else {
				address_count = parseInt(address_count);
			}
			for(i = 0; i<res.data.result.inputs.length; i++) {
				var key = localStorage.getItem('addrkey' + res.data.result.inputs[i][2]);
				key = webwallet.decrypt(pas, key);
				key = webwallet.ecpair.fromWIF(key, webwallet.network);
				psbt.signInput(i, key);
			}
			psbt.finalizeAllInputs();
			return psbt.extractTransaction(true);
		};
		
		webwallet.sending = false;
		webwallet.balance_is_loading = false;
		webwallet.transactions_are_loading = false;
		webwallet.updater_is_loading = false;

		webwallet.show_privatekey = function() {
			webwallet.keyindex = $(this).data('index');
			alertifyjs.show_privatekey_dialog($('#ask-password-show-privatekey-form')[0]);
		};

		webwallet.genericDialogOptions = function(){
			return {
				main:function(content){
					this.setContent(content);
				},
				setup:function(){
					return {
						focus:{
							element:function(){
								return this.elements.body.querySelector(this.get('selector'));
							},
							select:true
						},
						options:{
							basic:true,
							maximizable:false,
							resizable:false,
							padding:false
						}
					};
				},
				settings:{
					selector:undefined
				}
			};
		};

		window.mobileFooter = function(i18n){
		  var i18n = i18n ? i18n : { current: 'current'},
			  navs = document.querySelectorAll('nav.wfooter');
		  [].forEach.call( navs, function (nav, index) {
			var a = 0, c = 0, i = 1, // active, current, increment
			links = nav.getElementsByTagName('a'),
			 line = nav.querySelector('.line');      
			if ( !line ) {
			  line = document.createElement('i');
			  line.setAttribute('aria-hidden', true);
			  line.className = 'line';
			  line.innerHTML  = i18n.current;
			  nav.appendChild(line);
			}
			line.id = 'nav-current'+ index;
			if(!line.innerHTML.length) line.innerHTML = i18n.current;
			[].forEach.call(links, function (link, index) {   
			  link.removeAttribute('aria-describedby');
			  if (link.className.match(/\bactive\b/g)) place(line, link);
			  link.addEventListener('click', function (e) {
				a = index;
				var t = setInterval( function() {
				  links[c].classList.remove('traversing');
				  links[c].classList.remove('active');
				  if (a > c) i = 1;
				  else if (a < c) i = -1;
				  c += i;
				  links[c].classList.add('traversing');
				  if (c != -1) {
					links[c-i].classList.remove('active');
				  }
				  if (c == a) {
					e.target.classList.remove('traversing');
					e.target.classList.add('active');
					var page = document.getElementById(e.target.dataset.page);
					if(page !== null) {
						  var pages = document.querySelectorAll('.page');
						  [].forEach.call( pages, function (page, index) {
							  page.style.display = "none";
						  });
						page.style.display = "block";
					}
					i = 0;
					clearInterval(t);
				  }
				}, 100); // Traversing hilite: min: 100ms
				place(line, e.target);
			  });
			});
		  });
		  function place(l, a){
			a.setAttribute('aria-describedby', l.id || 'nav-current');
		/*     l.style.width = a.offsetWidth +'px'; */ 
			l.style.left  = a.offsetLeft + a.offsetWidth/2 +'px';
		  }
		};

		alertifyjs.dialog('show_privatekey_dialog',webwallet.genericDialogOptions);
		alertifyjs.dialog('create_address_dialog',webwallet.genericDialogOptions);
		alertifyjs.dialog('send_dialog',webwallet.genericDialogOptions);
		alertifyjs.dialog('privatekey_dialog',webwallet.genericDialogOptions);
		alertifyjs.dialog('recovery_key_dialog',webwallet.genericDialogOptions);
	},
	load() {
		window.onNuxtReady((app) => {
			mobileFooter();
			if (localStorage.getItem('key') == null) {
				$('.wfooter').css('display', 'none');
				$('#page-setpassword').css('display', 'block');
			} else {
				$('[data-page="page-home"]')[0].click();
				var address_count = localStorage.getItem('address_count');
				if(address_count === null) {
					address_count = 0;
				} else {
					address_count = parseInt(address_count);
				}
				var taddr;
				var addr_item;
				var i;
				for(i=0; i<address_count; i++) {
					var taddr = localStorage.getItem('addr' + i);
					var tcard = $('<div/>');
					tcard.addClass('card');
					$('#addrlist').append(tcard);
					
					var tcardcontent = $('<div/>');
					tcardcontent.addClass('card-content');
					tcard.append(tcardcontent);

					var tcontent = $('<div/>');
					tcontent.addClass('content');
					tcardcontent.append(tcontent);
					
					var tcolumns = $('<div/>');
					tcolumns.addClass('columns is-flex-direction-row is-flex-wrap-wrap');
					tcontent.append(tcolumns);
					
					var tcol = $('<div/>');
					tcol.addClass('column is-two-thirds');
					tcol.text(taddr);
					
					tcolumns.append(tcol);

					tcol = $('<div/>');
					tcol.addClass('column is-one-third');

					var tbtn = $('<button/>');
					tbtn.addClass('button is-primary is-rounded');
					tbtn.data('index', i);
					tbtn.text('Show Private Key');
					tbtn.click(webwallet.show_privatekey);
					tcol.append(tbtn);
					
					tcolumns.append(tcol);
				}
			}
		});
	},
  },
  head() {
    return {
      title: this.title,
      meta: [
      ],
      link: [
      ],
      script: [
        {
            src: '/fontawesome-free-6.4.2-web/js/all.min.js',
            type: 'text/javascript'
        }
      ]
    }
  }
}
</script>
