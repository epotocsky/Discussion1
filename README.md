def getData (name, cur, conn): 
    user_name = None
    screen_name = None
    follower_count = None

    try:
        new_name = name[1].lower().replace(" ", "")
        cursor = tweepy.Cursor(api.user_timeline, id = new_name, tweet_mode = 'extended').items(1)
        print(new_name)
 
        for i in cursor:
            # print(i)
            verified = i.author.verified
            # print(verified)
            user_name = i.author.name
            screen_name = i.author.screen_name
            follower_count = i.author.followers_count
            tweet_count = i.author.statuses_count    
    except:
        print("Cannot find data for this artist")


    if user_name != None and verified == True:
        print(follower_count)

        cur.execute('INSERT OR IGNORE INTO TwitterData (id, name, screen_name, follower_count) VALUES (?,?,?,?)', (name[0], user_name, screen_name, follower_count))
        conn.commit()
