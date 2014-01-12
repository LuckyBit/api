.. LuckyBit API documentation master file, created by
   sphinx-quickstart on Sun Jan 12 01:25:10 2014.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to LuckyBit API documentation!
======================================================

This is the API documentation for LuckyBit - http://luckyb.it

Introduction
------------

Querying games
--------------

/api/getgames
^^^^^^^^^^^^^

::
  
  curl http://luckyb.it/api/getgames
  

::
  
  {
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
  

/api/getgamebyname/<name>
^^^^^^^^^^^^^^^^^^^^^^^^^

::
  
  curl http://luckyb.it/api/getgamebyname/red
  

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

/api/getbetsbyaddress/<address>
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::
  
  curl http://luckyb.it/api/getbetsbyaddress/1JHP6cCrn7CzMDvMkqne77k6qUVq9DteoB
  

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
  


/api/getbetsbytxid/<txid>
^^^^^^^^^^^^^^^^^^^^^^^^^

::
  
  curl http://luckyb.it/api/getbetsbytxid/laec4a5bfdf5de4e40f875b4fc7b5a08d0c82e01966a718790daa519e6e80fff
  


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
  

/api/getbetbytxidvout/<txidvout>
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::
  
  curl http://luckyb.it/api/getbetbytxidvout/1aec4a5bfdf5de4e40f875b4fc7b5a08d0c82e01966a718790daa519e6e80fff:3 
  

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
  

Querying keys and hashes
------------------------

/api/getcurrenthash
^^^^^^^^^^^^^^^^^^^

::
  
  curl http://luckyb.it/api/getcurrenthash
  

::
  
  {
    "2014-01-12": "1892e0e8235f470b79f1c99a9dec874ca345eb44da36b38ecb1d925981d737c8"
  }
  

/api/gethashbydate/<date>
^^^^^^^^^^^^^^^^^^^^^^^^^

::
  
  curl http://luckyb.it/api/gethashbydate/2013-11-13
  

::
  
  {
    "2013-11-13": "bd1cdea0ec811eac0debfcba6c2155207ab37a4cf9c1c59981dd2e534f5962a2"
  }
  

/api/getkeybydate/<date>
^^^^^^^^^^^^^^^^^^^^^^^^


::
  
  curl http://luckyb.it/api/getkeybydate/2013-11-13
  

::
  
  {
    "2013-11-13": "f8cbabfe1d051eca2ee607477d35ed3271e2fd39354f09b79187c8af7694c959"
  }
  
