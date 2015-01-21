.. LuckyBit API documentation master file, created by
   sphinx-quickstart on Sun Jan 12 01:25:10 2014.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to the LuckyBit API documentation!
=======================================================

This is the API documentation for LuckyBit - http://luckyb.it

Introduction
------------

The LuckyBit API allows read-only access to the LuckyBit database.
It aims at facilitating the creation of tools such as betting bots, verifiers
and other add-on software.

The API is a REST API using JSON as dataformat. It is available anonymously
and does not require identification or authentification.

This documentation has also been published at: https://github.com/LuckyBit/api

Example
-------

An example script has been published here: https://github.com/LuckyBit/bet-verifier
This script shows how to compute the hashes of a bet in order to prove that
any given bet is provably fair. The script uses this API and http://blockchain.info
for an impartial data source.

Querying games
--------------

Allows to query for active games. There are at least four active games at
any moment in time, namely *blue*, *green*, *yellow*, and *red*. A special 
*promo* game might be active and thus listed for special occasions. Non-active games
(different multipliers, special offers etc) are not listed.

Returns a dictionary of all active games. Every game is identified by it's name.
It has an ``address``, which is the game's associated bitcoin address, a ``min_amount``
and ``max_amount``, which express the game's valid range for betting in bitcoin, a list of 
``multipliers``, ordered by ``rank`` (*rank = 0* is in the middle and *rank = 8* is
at the border of the multiplier bar).

Getting all games
^^^^^^^^^^^^^^^^^

URL scheme: ``http://luckyb.it/api/getgames``

Returns a dictionary of all three active games.

Example call for querying (command line):

::
  
  curl http://luckyb.it/api/getgames
  

Example return data (JSON):

::
  
  {
    "blue": {...},
    "green": {
      "address": "1LuckyG4tMMZf64j6ea7JhCz7sDpk6vdcS", 
      "max_amount": 1.0, 
      "min_amount": 0.0005, 
      "multipliers": {
        "0": 0.4, 
        "1": 1.0, 
        "2": 1.1, 
        "3": 1.2, 
        "4": 1.4, 
        "5": 2.0, 
        "6": 3.0, 
        "7": 5.0, 
        "8": 22.0
      }
    }, 
    "yellow": {...},
    "red": {...}
  }
  

Get specific game by name
^^^^^^^^^^^^^^^^^^^^^^^^^

URL scheme: ``http://luckyb.it/api/getgamebyname/<name>``

Returns the game with the name ``name``. 
Returns ``{}`` if no game by this name found.

Example call for querying (command line):

::
  
  curl http://luckyb.it/api/getgamebyname/red
  

Example return data (JSON):

::
  
  {
    "red": {
      "address": "1LuckyR1fFHEsXYyx5QK4UFzv3PEAepPMK", 
      "max_amount": 0.02, 
      "min_amount": 0.001, 
      "multipliers": {
        "0": 0.2, 
        "1": 0.2, 
        "2": 0.2, 
        "3": 2.0, 
        "4": 4.0, 
        "5": 9.0, 
        "6": 24.0, 
        "7": 130.0, 
        "8": 999.0
      }
    }
  }
  

Querying bets
-------------

Allows to query bets from LuckyBit. Only bets that have been published (i.e., displayed
on the website) are listed. 

In general, bets are identified by ``txid:vout``, with ``txid`` being the transaction id
and ``vout`` being the number of the output in the description.

Field description:
 * ``bet_amount``: amount of bet, in bitcoin
 * ``binary_string``: movement of coin in triangle encoded as *0 = left* and *1 = right*
 * ``created_at``: timestamp of creation
 * ``game_address``: address of the game the bet was sent to
 * ``game_name``: name of the game the bet was sent to
 * ``multiplier_obtained``: multiplier obtained by the bet (depends on game)
 * ``payout_amount``: payout amount in bitcoin
 * ``player_address``: address from which the bet was sent from
 * ``txin_id``: transaction id of bet
 * ``txin_vout``: vout of bet in transaction ``txin_id``
 * ``txout_id``: id of payout transaction
 * ``type``: type of bet, either ``VALID_BET`` or ``INVALID_MIN`` or ``INVALID_MAX``


Result pagination
^^^^^^^^^^^^^^^^^

Bet results are paginated, that is, at maximum LuckyBit returns 100 bets on one ``page`` of results. Developers have to select pages in order to browse through all available results.

Parameter description:
 * ``limit``: limit the amount of values returned (min 1, max 100)
 * ``page``: allows to access a certain page of the result set

In case the last page has been surpassed, LuckyBit returns an empty result set (``{}``).

Example parameters that return the results 61-70 of all bets:

::

  curl http://luckyb.it/api/getbets?limit=10&page=6


Get all bets
^^^^^^^^^^^^

URL scheme: ``http://luckyb.it/api/getbets``

Gets all bets. Returns a dictionary in which bets are identified by ``txid:vout``.

Example call for querying (command line):

::
  
  curl http://luckyb.it/api/getbets
    

Example return data (JSON):

::
  
  {
    "0a7746ffb8f4afaf06832dfd4de205ed23f85fb40adb4d71509e0520b66b4eae:0": {
      "bet_amount": 0.00528974, 
      "binary_string": "1110000001110100", 
      "created_at": "2014-11-25 19:16:30", 
      "game_address": "1LuckyY9fRzcJre7aou7ZhWVXktxjjBb9S", 
      "game_name": "yellow", 
      "multiplier_obtained": 0.5, 
      "payout_amount": 0.00264487, 
      "player_address": "1CzpDGmLu1yeEFa9qquMVpDUhNv9gKoR7S", 
      "txin_id": "0a7746ffb8f4afaf06832dfd4de205ed23f85fb40adb4d71509e0520b66b4eae", 
      "txin_vout": 0, 
      "txout_id": "c6373983dc4f8015fb435f22a95e398fb0c85155d3e5e2363b3c8bfd1ee28785", 
      "type": "VALID_BET"
    }, 
    [...]
  }



Get bets by sender address
^^^^^^^^^^^^^^^^^^^^^^^^^^

URL scheme: ``http://luckyb.it/api/getbetsbyaddress/<address>``

Gets all bets sent from the specific bitcoin address ``address``. Returns a dictionary in which
bets are identified by ``txid:vout``.

.. NOTE::
  You can search by substring, that is, it is sufficient to specify a 
  part of the ``address`` you are looking for.

.. NOTE::
  The search is case-sensitive.

Example call for querying (command line):

::
  
  curl http://luckyb.it/api/getbetsbyaddress/1JHP6cCrn7CzMDvMkqne77k6qUVq9DteoB
  

Example return data (JSON):

::
  
  {
    "1aec4a5bfdf5de4e40f875b4fc7b5a08d0c82e01966a718790daa519e6e80fff:0": {
      "bet_amount": 0.001, 
      "binary_string": "0100000100000000", 
      "created_at": "2013-11-13 20:39:46", 
      "game_address": "1LuckyG4tMMZf64j6ea7JhCz7sDpk6vdcS", 
      "game_name": "green", 
      "multiplier_obtained": 3, 
      "payout_amount": 0.003, 
      "player_address": "1JHP6cCrn7CzMDvMkqne77k6qUVq9DteoB", 
      "txin_id": "1aec4a5bfdf5de4e40f875b4fc7b5a08d0c82e01966a718790daa519e6e80fff", 
      "txin_vout": 0, 
      "txout_id": "2554b98311479428a714ed930988e016cc93ece05ba21b64b9be9311a28854ce", 
      "type": "VALID_BET"
    }, 
    "1aec4a5bfdf5de4e40f875b4fc7b5a08d0c82e01966a718790daa519e6e80fff:2": {...},
    "1aec4a5bfdf5de4e40f875b4fc7b5a08d0c82e01966a718790daa519e6e80fff:3": {...},
    "7b35ae0cf8e8e7a94bd845eb31947736ccca99bd83795f3b30014ad0aa14f561:0": {...},
    "c3021f1231f61e2f735c49ba6f4d9e2506a8bbb945a16e0541c29fd1ee976abd:0": {...}
  }
  


Get bets by transaction id
^^^^^^^^^^^^^^^^^^^^^^^^^^

URL scheme: ``http://luckyb.it/api/getbetsbytxid/<txid>``

Gets all bets sent in a specific bitcoin transaction, identified by ``txid``.
Returns a dictionary in which bets are identified by ``txid:vout``.

.. NOTE::
  You can search by substring, that is, it is sufficient to specify a 
  part of the ``txid`` you are looking for.

.. NOTE::
  The search is case-sensitive.

Example call for querying (command line):

::
  
  curl http://luckyb.it/api/getbetsbytxid/laec4a5bfdf5de4e40f875b4fc7b5a08d0c82e01966a718790daa519e6e80fff
  

Example return data (JSON):

::
  
  {
    "1aec4a5bfdf5de4e40f875b4fc7b5a08d0c82e01966a718790daa519e6e80fff:0": {
      "bet_amount": 0.001, 
      "binary_string": "0100000100000000", 
      "created_at": "2013-11-13 20:39:46", 
      "game_address": "1LuckyG4tMMZf64j6ea7JhCz7sDpk6vdcS", 
      "game_name": "green", 
      "multiplier_obtained": 3, 
      "payout_amount": 0.003, 
      "player_address": "1JHP6cCrn7CzMDvMkqne77k6qUVq9DteoB", 
      "txin_id": "1aec4a5bfdf5de4e40f875b4fc7b5a08d0c82e01966a718790daa519e6e80fff", 
      "txin_vout": 0, 
      "txout_id": "2554b98311479428a714ed930988e016cc93ece05ba21b64b9be9311a28854ce", 
      "type": "VALID_BET"
    }, 
    "1aec4a5bfdf5de4e40f875b4fc7b5a08d0c82e01966a718790daa519e6e80fff:2": {...},
    "1aec4a5bfdf5de4e40f875b4fc7b5a08d0c82e01966a718790daa519e6e80fff:3": {...},
    "7b35ae0cf8e8e7a94bd845eb31947736ccca99bd83795f3b30014ad0aa14f561:0": {...},
    "c3021f1231f61e2f735c49ba6f4d9e2506a8bbb945a16e0541c29fd1ee976abd:0": {...}
  }
  

Get single bet by txit:vout
^^^^^^^^^^^^^^^^^^^^^^^^^^^

URL scheme: ``http://luckyb.it/api/getbetbytxidvout/<txid>:<vout>``

Gets a single bet, that is, a specific vout in a specific transaction, identified by ``txid:vout``.
Returns a dictionary in which bets are identified by ``txid:vout``.

.. NOTE::
  This call gets a single bet, and is called therefore ``/api/getbet`` rather than ``/api/getbets``

.. NOTE::
  The search is case-sensitive.

Example call for querying (command line):

::
  
  curl http://luckyb.it/api/getbetbytxidvout/1aec4a5bfdf5de4e40f875b4fc7b5a08d0c82e01966a718790daa519e6e80fff:3 
  

Example return data (JSON):

::
  
  {
    "1aec4a5bfdf5de4e40f875b4fc7b5a08d0c82e01966a718790daa519e6e80fff:3": {
      "bet_amount": 0.001, 
      "binary_string": "0100000110001011", 
      "created_at": "2013-11-13 20:39:46", 
      "game_address": "1LuckyR1fFHEsXYyx5QK4UFzv3PEAepPMK", 
      "game_name": "red", 
      "multiplier_obtained": 0.2, 
      "payout_amount": 0.0002, 
      "player_address": "1JHP6cCrn7CzMDvMkqne77k6qUVq9DteoB", 
      "txin_id": "1aec4a5bfdf5de4e40f875b4fc7b5a08d0c82e01966a718790daa519e6e80fff", 
      "txin_vout": 3, 
      "txout_id": "bb58d6ae2fa02a04b819bc7422d1210f32a69da25c994389bc07e9ec531aac44", 
      "type": "VALID_BET"
    }
  }
  

Get bets by date
^^^^^^^^^^^^^^^^

URL scheme: ``http://luckyb.it/api/getbetsdate/<date>``

Gets all bets sent on a specific date, identified by ``date`` using the format ``YYYY-MM-DD``
Returns a dictionary in which bets are identified by ``txid:vout``.

Example call for querying (command line):

::
  
  curl http://luckyb.it/api/getbetsbydate/2013-12-31


Example return data (JSON):

::

  {
    "0005a10e440fb736d307f1c275be46ad3769ff9738f726703424ea3a1aea8f62:0": {
      "bet_amount": 0.001, 
      "binary_string": "1100110011111110", 
      "created_at": "2013-12-31 04:25:15", 
      "game_address": "1LuckyR1fFHEsXYyx5QK4UFzv3PEAepPMK", 
      "game_name": "red", 
      "multiplier_obtained": 2, 
      "payout_amount": 0.002, 
      "player_address": "1Bqbu2rgJVWfw1aAw3VM98JBkNdE9Cuw4G", 
      "txin_id": "0005a10e440fb736d307f1c275be46ad3769ff9738f726703424ea3a1aea8f62", 
      "txin_vout": 0, 
      "txout_id": "a6ca48610622db2cc1a34a0ba527008bfac5c349f4208b0cb070c6d9aa1ae30d", 
      "type": "VALID_BET"
    }, 
    [...]
  }
  

Querying keys and hashes
------------------------

Allows to query secret keys and their hashes. All secret keys
are strings 64 characters long. Hashes are SHA256 hashes of
these strings.

Get todays hash
^^^^^^^^^^^^^^^

URL scheme: ``http://luckyb.it/api/getcurrenthash``

Returns the hash of the currently used secret key, identified by the date.

Example call for querying (command line):

::
  
  curl http://luckyb.it/api/getcurrenthash
  

Example return data (JSON):

::
  
  {
    "2014-01-12": "1892e0e8235f470b79f1c99a9dec874ca345eb44da36b38ecb1d925981d737c8"
  }
  

Get hash by date 
^^^^^^^^^^^^^^^^

URL scheme: ``http://luckyb.it/api/gethashbydate/<date>``

Returns the hash of the day ``date``, identified by the date.

Example call for querying (command line):

::
  
  curl http://luckyb.it/api/gethashbydate/2013-11-13
  

Example return data (JSON):

::
  
  {
    "2013-11-13": "bd1cdea0ec811eac0debfcba6c2155207ab37a4cf9c1c59981dd2e534f5962a2"
  }
  

Get secret key by date
^^^^^^^^^^^^^^^^^^^^^^

URL scheme: ``http://luckyb.it/api/getkeybydate/<date>``

Returns the secret key of a specific date, identified by ``date`` using the format ``YYYY-MM-DD``

.. NOTE::
  You can only query the secret keys starting from yesterday.

Example call for querying (command line):

::
  
  curl http://luckyb.it/api/getkeybydate/2013-11-13
  

Example return data (JSON):

::
  
  {
    "2013-11-13": "f8cbabfe1d051eca2ee607477d35ed3271e2fd39354f09b79187c8af7694c959"
  }
 

Contact
-------

 * Follow us on Twitter: https://twitter.com/LuckyBitGame
 * Bitcointalk support thread: https://bitcointalk.org/index.php?topic=757624
 * LuckyBit support: support@luckyb.it

