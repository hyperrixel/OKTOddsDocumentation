# OKTOdds documentation

[The project has a devpost page](https://devpost.com/software/oktodds)

[The project has a YouTube peresentation](https://youtu.be/PJ-VJRaOOV8)

This smart contract is deployed to the thestnet address: 0x3Ad64B7087e39ffa18a9B27ad4dd1B61b7c283d1

SPDX-License-Identifier: Copyright
pragma solidity >=0.7.0 <0.9.0;


@title OKTOdds
@dev OKTOdds

# contract OKTOdds

#### Smart Contract Constructor

@param sentence A sentence to increase contract's security

constructor(string memory sentence) payable {}

## Raw data manipulation of users

### CREATE

#### Create a user record

@param displayName Name of the user to display
@param personalMesssage A personal message to show (moto, webpage, etc.)
@param favoriteSport Name of the favorite sport
@param favoriteTeam Name of the favorite team
@param favoritePlayer Name of the favorite player

function register (string calldata displayName, string calldata personalMesssage, string calldata favoriteSport, string calldata favoriteTeam, string calldata favoritePlayer) onlyNewUser(msg.sender) external {}


#### Create a user record


@param displayName Name of the user to dsiplay
@param personalMesssage A personal message to show (moto, webpage, etc.)
@param favoriteSport Name of the favorite sport
@param favoriteTeam Name of the favorite team
@param favoritePlayer Name of the favorite player

function register (string calldata displayName, string calldata personalMesssage, string calldata favoriteSport, string calldata favoriteTeam, string calldata favoritePlayer, address referrer) onlyNewUser(msg.sender) onlyExistingUser(referrer) external {}


### READ

// Entity level get() is implied since the variable users is declared public.

// listUsers() missing due to limitations of solidity mapping return

// FUTURE:
//
// getUsersByDisplayName(name)
// getUsersByFavoriteSport(sport)
// getUsersByFavoriteTeam(team)
// getUsersByFavoritePlayer(player)

### UPDATE

#### Modify a user record

@param displayName Name of the user to dsiplay
@param personalMesssage A personal message to show (moto, webpage, etc.)
@param favoriteSport Name of the favorite sport
@param favoriteTeam Name of the favorite team
@param favoritePlayer Name of the favorite player

function changeUser (string calldata displayName, string calldata personalMesssage, string calldata favoriteSport, string calldata favoriteTeam, string calldata favoritePlayer, string calldata) onlyExistingUser(msg.sender) external {}


## Suspend user

function suspend() onlyExistingUser(msg.sender) external {}


### DELETE


#### Request erase of the user

function requestDeleteUser() onlyExistingUser(msg.sender) external {}


#### Request erase of a user

@param targetUser User to erase
@param reason Reason to erase user

function requestReportUser(address targetUser, tring calldata reason) onlyExistingUser(targetUser) external {}


#### Get full list of user erase requests

@param sentence Admin credentials

function listUserReportRequests(string calldata sentence) onlyAdmin(sentence) external view returns  (UserReportRequest[] memory) {}

#### Change state of a user

@param sentence Admin credentials
@param userAddress Address of the user to change state for
@param newStatus State to set

function changeUserState(string calldata sentence, address userAddress, uint8 newStatus) onlyAdmin(sentence) onlyExistingUser(userAddress) public {}


#### Change state of a user and delete request id

@param sentence Admin credentials
@param userAddress Address of the user to change state for
@param newStatus State to set
@param requestID ID of the request

function changeUserState(string calldata sentence, address userAddress, uint8 newStatus,  uint requestID) onlyAdmin(sentence) onlyExistingUser(userAddress) external {}


## Raw data manipulation of events

### CREATE

#### Add event

@param sentence Admin credentials
@param title Title of the event
@param sport Type of sport of the event
@param homeEntity Home team/player of the match
@param guestEntity Guest team/player of the match

function addEvent(string calldata sentence, string calldata title, string calldata sport, string calldata homeEntity, string calldata guestEntity, uint8 resultClass, uint256 expiration, string calldata website, string calldata resultWebsite) onlyAdmin(sentence) external {}

// FUTURE:
//
// regestEventToAdd()


## READ

// Entity level get() is implied since the variable events is public.

// listEvents() missing due to limitations of solidity mapping return

// FUTURE:
//
// getEventsByTitle(title)
// getEventsBySport(sport)
// getEventsByEntity(entity)
// getEventsByExpiration(expiration)
// getEventsByWebsite(website)
// getEventsByResultWebsite(resultWebsite)
// getEventsByResult(expectedResult)

#### Get last used event ID


function getLastEventId() external view returns (uint32) {}


### UPDATE

#### Update event's textual content

@param sentence Admin credentials
@param eventId Id of the event to update
@param title Title of the event
@param sport Type of sport of the event
@param homeEntity Home team/player of the match
@param guestEntity Guest team/player of the match

function changeEventBaseData(string calldata sentence, uint32 eventId, string calldata title, string calldata sport, string calldata homeEntity, string calldata guestEntity) onlyAdmin(sentence) onlyExistingEvent(eventId) external {}


#### Update event's expiration

@param sentence Admin credentials
@param eventId Id of the event to update
@param expiration Expiration of the event

function changeEventExpiration(string calldata sentence, uint32 eventId, uint256 expiration) onlyAdmin(sentence) onlyLivingEvent(eventId) external {}


#### Update event's website data

@param sentence Admin credentials
@param eventId Id of the event to update
@param website Website of the event
@param resultWebsite Website of the event's results

function changeEventWebsites(string calldata sentence, uint32 eventId, string calldata website, string calldata resultWebsite) onlyAdmin(sentence) onlyExistingEvent(eventId) external {}


#### DELETE




#### Delete event

@param sentence Admin credentials
@param eventId Id of the event to update

function deleteEvent(string calldata sentence, uint32 eventId) onlyAdmin(sentence) onlyLivingEvent(eventId) external {}


## Raw data manipulation of bets

### CREATE

// Direct create not available, not needed.
// If needed, it should be in addEvent() function


### READ

// Entity level get() is implied since the variable bets is declared public.

// listBets() missing due to limitations of solidity mapping return

// FUTURE:
//
// getBetsByEventAndExpectedResult()


### UPDATE

#### Make a bet

@param eventId The ID of the event to bet
@param expectedResult The result that is expected as winner
@param stake The amount of stake to bet with

function placeBet(uint32 eventId, uint8 expectedResult, uint256 stake) onlyExistingUser(msg.sender) onlyLivingEvent(eventId) onlyValidResult(eventId, expectedResult) external payable {}


### DELETE

// No need to delete a bet.

## MISCELLANEOUS FUNCTIONS


## Get user's balance


function getMyBalance() external view returns (uint256) {}


## Withdraw


@param amount Amount to withdraw, if 0 is set, it sends the whole balance

function withdraw(uint256 amount) external payable {}


#### Close event

@param sentence Admin credentials
@param eventId The ID of the event to close
@param when Time to close, if 0 is given, the event gets closed now

function closeEvent(string calldata sentence, uint32 eventId, uint256 when) onlyAdmin(sentence) onlyLivingEvent(eventId) external {}


#### Finalize event

@param sentence Admin credentials
@param eventId The ID of the event to close
@param result The final result of the event

function finalizeEvent(string calldata sentence, uint32 eventId, uint8 result) onlyAdmin(sentence) onlyValidFinalResult(eventId, result) external {}


#### Get current odds

@param eventId The ID of the event to give the actual odds for

function getCurrentRates(uint32 eventId) onlyExistingEvent(eventId) external view returns (uint256, uint256, uint256) {}


## INTERNAL FUNCTIONS

#### Get fee for the given amount


@param amount The amount to calculate fee from

function feeFunction(uint256 amount) internal pure returns (uint256) {}


#### Calculate percentage


@param amount Amount to use in calculation
@param percent Percent to use in calculation

function getPercent(uint256 amount, uint256 percent) internal pure returns (uint256) {}


#### Get rates

function getRates(uint32 eventId) internal view returns (uint256 homeRate, uint256 guestRate, uint256 drawRate) {}


#### Check whether a user is registered

@param userAddress The address of the user to check

function isUser(address userAddress) internal returns (bool isRegistered) {}


#### Remove registered user

@param userAddress User to remove

function removeUser(address userAddress) internal {}


### MODIFIERS

#### Modifier to optimize code if checking admin credentials


@param sentence Admin credentials

modifier onlyAdmin(string memory sentence) {}


#### Modifier to optimize code if checking event's existence

@param eventId The ID of the event to check

modifier onlyExistingEvent(uint32 eventId) {}


#### Modifier to optimize code if checking user's existence

@param userAddress The address of the user to check

modifier onlyExistingUser(address userAddress) {}


#### Modifier to optimize code if checking event's expiration

@param eventId The ID of the event to check

modifier onlyLivingEvent(uint32 eventId) {}


#### Modifier to optimize code if checking the lack of user's existence

@param userAddress The address of the user to check

modifier onlyNewUser(address userAddress) {}


#### Modifier to optimize code if checking the validity of a final result

@param eventId The ID of the event to check
@param finalResult The ID of the expected result

modifier onlyValidFinalResult(uint32 eventId, uint8 finalResult) {}


#### Modifier to optimize code if checking the validity of a result

@param eventId The ID of the event to check
@param expectedResult The ID of the expected result

modifier onlyValidResult(uint32 eventId, uint8 expectedResult) {}


## STRUCT DEFINITIONS

struct User {string displayName; string personalMesssage; string favoriteSport; string favoriteTeam; string favoritePlayer; uint8 status;}

struct Referrer {address registered; address referrer;}

struct Event {string title; string sport; string[2] entities; uint8 resultClassId; uint256 expiration; string website; string resultWebsite;}

struct Bet {uint16 expectedResult; address user; uint256 stake;}

struct Balance {address user; uint256 amount;}

struct EventReportRequest {uint32 eventId; address sender; string text;}

struct UserReportRequest {address target; address sender; string text;}


## PUBLIC CONSTANTS

#### Users
uint8 constant public USER_DELETED = 0;
uint8 constant public USER_ACTIVE = 1;
uint8 constant public USER_BANNED = 2;
uint8 constant public USER_SUSPENDED = 3;

#### Events
uint8 constant public EVENT_NOT_SET = 0;
uint8 constant public EVENT_DELETED = 1;
uint8 constant public EVENT_ACTIVE = 2;
uint8 constant public EVENT_RESULT_HOME_WIN = 3;
uint8 constant public EVENT_RESULT_GUEST_WIN = 4;
uint8 constant public EVENT_RESULT_DRAW = 5;

#### Result classes
uint8 constant public RESULT_CLASS_NOT_SET = 0;
uint8 constant public RESULT_CLASS_ONE_WINS = 1;
uint8 constant public RESULT_CLASS_WIN_OR_DRAW = 2;

#### Miscellaneous
string constant public CONTRACT_VERSION = '0.1.0';
string constant public FEE_RULE_DESCRIPTION = 'fee = 1 % of bet stake';
uint256 constant public FEE_PERCENTAGE = 1;
uint256 constant public MAXIMUM_WIN_RATE_PERCENTAGE = 1000; // 10 x win
uint256 constant public MINIMUM_BET_STAKE = 100;
uint256 constant public MINIMUM_WITHDRAW_AMOUNT = 10000;


## STATE VARIABLES

#### Admin credentials
address payable rootUser;
bytes32 rootKey;

#### Public variables
mapping (address => User) public users;
mapping (uint32 => Event) public events;
mapping (uint32 => uint8) public results;
mapping (uint32 => Bet[]) public bets;

#### Private variables
address[] private registeredUsers;
UserReportRequest[] private userReportRequests;
uint32 private nextEventID;
mapping (address => uint256) private balances;
Referrer[] private referrers;
