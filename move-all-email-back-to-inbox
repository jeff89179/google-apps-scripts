// This script moves all emails back to the inbox, removes any other labels applied and marks them as unread.
// Runs in batches of 100 and keeps track of where it left off so it can pick up again when it hits the API time limit.

function cleanUpEmails() {
  // Get the user's email
  var userEmail = Session.getActiveUser().getEmail();

  // Get the script's user properties
  var scriptProperties = PropertiesService.getUserProperties();
  
  // If a start index isn't stored, start at 0
  var startIndex = parseInt(scriptProperties.getProperty('START_INDEX')) || 0;
  
  // Process in batches of 100
  var batchSize = 100;

  // Get the Gmail threads
  var threads = GmailApp.getInboxThreads(startIndex, batchSize);

  while(threads.length > 0) {
    // For each thread
    for(var i = 0; i < threads.length; i++) {
      var thread = threads[i];
      
      // Remove all labels from the thread
      var labels = thread.getLabels();
      for(var j = 0; j < labels.length; j++) {
        thread.removeLabel(labels[j]);
      }

      // Move the thread to the inbox
      if (!thread.isInInbox()) {
        thread.moveToInbox();
      }

      // Mark the thread as unread
      if (thread.isUnread() == false) {
        thread.markUnread();
      }
    }

    // Update the start index
    startIndex += threads.length;
    scriptProperties.setProperty('START_INDEX', startIndex.toString());

    // Get the next batch of threads
    threads = GmailApp.getInboxThreads(startIndex, batchSize);
  }
  
  // Once all threads have been processed, reset the start index
  scriptProperties.deleteProperty('START_INDEX');
}
