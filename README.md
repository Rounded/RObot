# Rounded Robot

[![CI Status](http://img.shields.io/travis/Brian Weinreich/RoundedRobot.svg?style=flat)](https://travis-ci.org/Brian Weinreich/RoundedRobot)
[![Version](https://img.shields.io/cocoapods/v/RoundedRobot.svg?style=flat)](http://cocoapods.org/pods/RoundedRobot)
[![License](https://img.shields.io/cocoapods/l/RoundedRobot.svg?style=flat)](http://cocoapods.org/pods/RoundedRobot)
[![Platform](https://img.shields.io/cocoapods/p/RoundedRobot.svg?style=flat)](http://cocoapods.org/pods/RoundedRobot)

## Usage

To run the example project, clone the repo, and run `pod install` from the Example directory first.

## Requirements

iOS 7 or greater

## Installation

RObot is available through [CocoaPods](http://cocoapods.org). To install
it, simply add the following line to your Podfile:

```ruby
pod "RoundedRobot"
```

## How to use

RObot a stand alone libraby that handles CRUD methods for `NSManagedObjects`. Calling create, read, update, delete, or index, will make the API and update the database appropriately.

To begin, call this line:

    [ROBotManager initializeWithBaseURL:@"https://your_base_url"];
    
If you plan on using the index methods or caching, you'll also need to set your persistent store coordinator:

    [ROBotManager sharedInstance].persistentStoreCoordinator = [self persistentStoreCoordinator];


Next, `#import "ROBot.h"` into the classes you plan on using ROBot in.

You'll need to override the CRUD urls in your `NSManagedObject` (or corresponding catorgory). Ex:

	+ (NSString *)indexURL
	{
    	return @"/users";
	}

	- (NSString *)createURL
	{
    	return @"/users";
	}

	- (NSString *)readURL
	{
    	return [NSString stringWithFormat:@"/users/%@", self.user_id];
	}

	- (NSString *)updateURL
	{
    	return [NSString stringWithFormat:@"/users/%@", self.user_id];
	}


Note that the url path are instance methods, and will populate the variables in the url string when the url methods is called.

You can then call CRUD methods on the instances of the model, and the changes will be made to your database and API. 

Get all Users:
	
	[User index:^{} failure:^(ROBotError *error) {}];

Create Example:

    WRUser *user = [NSEntityDescription insertNewObjectForEntityForName:@"WRUser" inManagedObjectContext:[NSManagedObjectContext MR_defaultContext]];
    user.fname = @"Heather";
    user.lname = @"Sneps";
    [user create:^{} failure:^(ROBotError *error) {}]


## Customization

You can specify a custom mapping from the API response to the database object. If you don't specify a mapping, RObot will map the results from the API directly to the corresponding fields in your database.

To specify a mapping, do the following:

  	[User setMapping:@[@"last_name": @"lname", @"first_name": @"fname"]];

Note that a partial mapping can be used. In the case above, the email will still map to email.


## Headers

To add custom headers to requests, do the following:

        [[ROBotManager sharedInstance] addHeaderValue:@"Hunting" forHeaderField:@"SL-Sport"];

You can add as many default headers as you would like.

## Logging

By default, logging is disabled. To enable logging for all your `ROBotManagedObjects`, use 

  	ROBotManagedObject.verboseLogging = true

to enable logging for a single `ROBotManagedObject` class, use

  User.verboseLogging = true

## Thanks!

Feel free to submit a pull request and add your own contributions!



## Author

- Heather Snepenger, hs@roundedco.com (WAAH WAAAH)
- Brian Weinreich, bw@roundedco.com (Mr. Amazing)

## License

RObot is available under the MIT license. See the LICENSE file for more info.
