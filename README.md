import twitter4j.*;
import twitter4j.conf.ConfigurationBuilder;
import java.util.List;

public class TwitterDataCollector {

    public static void main(String[] args) {

        // Twitter API credentials
        String consumerKey = "your_consumer_key";
        String consumerSecret = "your_consumer_secret";
        String accessToken = "your_access_token";
        String accessTokenSecret = "your_access_token_secret";

        // Configure Twitter4J with API credentials
        ConfigurationBuilder cb = new ConfigurationBuilder();
        cb.setDebugEnabled(true)
                .setOAuthConsumerKey(consumerKey)
                .setOAuthConsumerSecret(consumerSecret)
                .setOAuthAccessToken(accessToken)
                .setOAuthAccessTokenSecret(accessTokenSecret);

        // Create TwitterFactory
        TwitterFactory tf = new TwitterFactory(cb.build());
        Twitter twitter = tf.getInstance();

        // Query for CAA related tweets
        Query query = new Query("Citizenship Amendment Act");
        query.setCount(100); // Set number of tweets to retrieve
        query.setLang("en"); // Set language to English

        try {
            QueryResult result = twitter.search(query);
            List<Status> tweets = result.getTweets();

            // Process each retrieved tweet
            for (Status tweet : tweets) {
                // Extract relevant information from tweet
                String username = tweet.getUser().getScreenName();
                String tweetText = tweet.getText();
                // Add further processing or storage logic here
                System.out.println("@" + username + ": " + tweetText);
            }
        } catch (TwitterException te) {
            te.printStackTrace();
            System.out.println("Failed to search tweets: " + te.getMessage());
        }
    }
}
