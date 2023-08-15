<!--
 _       __          __  _____
| |     / /__  ___  / /_|__  /
| | /| / / _ \/ _ \/ //_//_ < 
| |/ |/ /  __/  __/ ,< ___/ / 
|__/|__/\___/\___/_/|_/____/  
                              
Maintainers:
Date Created:
-->

# GroupWork2

<details>
<summary>Refactored or Created Files</summary>


<details>
<summary>Group Work</summary>

<details>
<summary>WeeklyRosterManager</summary>

```java
package com.kenzie.attendance.management;

import com.kenzie.attendance.participants.Participant;
import com.kenzie.attendance.participants.ParticipantManager;
import com.kenzie.set.Set;

import java.time.LocalDate;
import java.util.Iterator;

/**
 * A tool to help ATA program managers track a week's attendance and swaps.
 */
public class WeeklyRosterManager {
    // the monday of the week the classes will be held
    private LocalDate mondayOfTeachingWeek;
    // a class with knowledge about each participant, linking their participant id with name and alias data
    private ParticipantManager manager;

    private int maxClassSize = 60;

    Set<Long> tuesdayParticipants = new Set<>(maxClassSize);
    Set<Long> thursdayParticipants = new Set<>(maxClassSize);




    // The maximum swap size
    private int maxSwapSize = 5;
    private int numTuesSwaps = 0;
    private int numThursSwaps = 0;

    Set<Long> tuesdaySwapIns = new Set<>(maxSwapSize);
    Set<Long> thursdaySwapIns = new Set<>(maxSwapSize);


    /**
     * Instantiates a new Weekly roster manager.
     *
     * @param weekOfDate         the week of date
     * @param participantManager the participant manager
     */
    public WeeklyRosterManager(LocalDate weekOfDate, ParticipantManager participantManager) {
        mondayOfTeachingWeek = weekOfDate;
        manager = participantManager;
    }

    /**
     * The entry point of application.
     *
     * @param args the input arguments
     */
    public static void main(String[] args) {

        WeeklyRosterManager sectionManager = new WeeklyRosterManager(LocalDate.of(2019, 8, 13),
                                                                     new ParticipantManager());
        // new Participant Manager will provide access of a set containing (long) ID, Alias, Name

        System.out.println("Setting class rosters to standard sections.");
        sectionManager.addParticipantsToSection(SectionDay.TUESDAY, "1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17," +
            "18,19,20,21,22,23,24,25,26,27,28,29");

        sectionManager.addParticipantsToSection(SectionDay.THURSDAY, "30,31,32,33,34,35,36,37,38,39,40,41,42,43," +
            "44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59");


        sectionManager.printSectionRosters();



        System.out.println("\nMarking participants as absent.");
        // Mark Brian Oller absent from correct section
        boolean brianMarkedAbsent = sectionManager.scheduleAbsence(SectionDay.TUESDAY, 7);
        checkEquals(true, brianMarkedAbsent,
                    "ERROR: Unsuccessful in marking Participant with id: 7 as absent from " + SectionDay.TUESDAY);
        System.out.println("Participant with id: 7 marked as absent from " + SectionDay.TUESDAY);

        // Mark Caleb Santana absent from correct section
        boolean calebMarkedAbsent = sectionManager.scheduleAbsence(SectionDay.THURSDAY, 37);
        checkEquals(true, calebMarkedAbsent,
                    "ERROR: Unsuccessful in marking Participant with id: 37 as absent from " + SectionDay.THURSDAY);
        System.out.println("Participant with id: 37 marked as absent from " + SectionDay.THURSDAY);

        // Unable to mark Mwila Mwila absent from wrong section
        boolean mwilaMarkedAbsent = sectionManager.scheduleAbsence(SectionDay.TUESDAY, 57);
        checkEquals(false, mwilaMarkedAbsent,
                    "ERROR: Successful in marking Participant with id: 57 as absent in wrong section.");

        // COMPLETION FINISHED

        // EXTENSION - uncomment the below code blocks to test your swap logic

         System.out.println("\nPerforming participant swaps.");
        // Swap Subin Hong from the Tuesday section to the Thursday section
         boolean subinSwapped = sectionManager.scheduleSwap(SectionDay.TUESDAY, 22);
         checkEquals(true, subinSwapped, "ERROR: Unsuccessful in swapping participant with id: 22 into: " +
                     SectionDay.THURSDAY);
        System.out.println("Participant with id: 22 swapped into " + SectionDay.THURSDAY);

        // Swap Kundan Rai from the Thursday section to the Tuesday section
         boolean kundanSwapped = sectionManager.scheduleSwap(SectionDay.THURSDAY, 36);
         checkEquals(true,kundanSwapped, "ERROR: Unsuccessful in swapping participant with id: 36 into: " +
                      SectionDay.TUESDAY);
         System.out.println("Participant with id: 36 swapped into " + SectionDay.TUESDAY);

        // Unable to swap Theta Milhoan from the wrong section
         boolean thetaSwapped = sectionManager.scheduleSwap(SectionDay.THURSDAY, 8);
         checkEquals(false, thetaSwapped, "ERROR: Successful in swapping participant with id: 8 into section " +
                    "they already attend");

        System.out.println("\nPrinting section rosters.\n");
        sectionManager.printSectionRosters();
    }

    private static void checkEquals(boolean expected, boolean actual, String errorMessage) {
        if (expected != actual) {
            System.out.println(errorMessage);
            System.exit(1);
        }
    }

    /**
     * Helper method to choose the appropriate attendance set.
     * @param section The day of attendance.
     * @return The Set corresponding to the section.
     */
    public Set<Long> getSectionAttendees(SectionDay section) {
        Set<Long> sectionAttendees;
        if (section == SectionDay.TUESDAY) {
            sectionAttendees = tuesdayParticipants;
        } else if (section == SectionDay.THURSDAY) {
            sectionAttendees = thursdayParticipants;
        } else {
            throw new IllegalArgumentException(String.format(
                    "Can't get attendees for unknown section day %s!", section.name()));
        }
        return sectionAttendees;
    }

    /**
     * COMPLETION.
     * <p>
     * Add a comma separated list of participant ids to the denoted section
     * PARTICIPANTS: hint 1 - Java String's split method may be useful here
     * hint 2 - Our pre-work introduced us to wrapper class methods that transform strings to numbers
     * hint 3 - Use your print statement debugging technique here to ensure you're correctly adding
     * the participants
     *
     * @param section         which section the participants should be added to, TUESDAY or THURSDAY
     * @param participantList a comma separated list of ids
     */
    // TODO                          # 1
    // TODO         Populate tuesdayParticipants and thursdayParticipants
    // TODO               these 2 sets contain only the student IDs
    // TODO                   IDs then used in printSectionRosters() to look up name & alias
    public void addParticipantsToSection(SectionDay section, String participantList) {
        String[] participantIDs = participantList.split(",");

        for(String participant: participantIDs){
            // convert String ID to long ID
            long pid = Long.parseLong(participant);
            if(section.equals(SectionDay.TUESDAY)){
                tuesdayParticipants.add(pid);
            } else if(section.equals(SectionDay.THURSDAY)){
                thursdayParticipants.add(pid);
            }
        }

    }

    // TODO                          # 2
    // TODO            Print Rosters of Tues & Thurs class

    public void printSectionRosters() {
       // System.out.println("\n\n Printing section rosters \n\n");
        // TODO PRINT TUESDAY PARTICIPANTS
        System.out.println(tuesdayParticipants.size() + " participants attending class on: TUESDAY the week of: " +
                mondayOfTeachingWeek + "\n\n");
        Iterator<Long> itr = tuesdayParticipants.iterator();
        while(itr.hasNext()){
            long id = itr.next();
            System.out.println(  manager.lookupParticipantById(id));
        }
        // TODO PRINT THURSDAY PARTICIPANTS
        System.out.println( "\n\n" + thursdayParticipants.size() + " participants attending class on: THURSDAY the week of: " +
                mondayOfTeachingWeek + "\n\n");
        Iterator<Long> itrThurs = thursdayParticipants.iterator();
        while(itrThurs.hasNext()){
            long id = itrThurs.next();
            System.out.println(  manager.lookupParticipantById(id));

        }

    }
    // TODO                          # 3
    // TODO            Remove student from roster when ABSENT
    // TODO               return TRUE if successful, FALSE otherwise
    /**
     * COMPLETION.
     * <p>
     * Remove a participant from the list of attendees for the class.
     *
     * @param attendedSection the section the participant will be absent from
     * @param participantId   the id of the participant that will be absent
     * @return true if the participant was successful removed, false otherwise
     */
    public boolean scheduleAbsence(SectionDay attendedSection, long participantId) {
        // Tuesday absence
        if(attendedSection.equals(SectionDay.TUESDAY)){
            // is the person in the Tuesday class?  if yes, remove them, return true
            if(tuesdayParticipants.contains(participantId)){
                tuesdayParticipants.remove(participantId);
                return true;
            // if they're not in the Tuesday class, return false
            } else if(!tuesdayParticipants.contains(participantId)){
                System.out.println("participant id " + participantId + " not found in the " + attendedSection + " class.");
                return false;
            }


        } else if(attendedSection.equals(SectionDay.THURSDAY)){
            // is person in the Thursday class?  if yes, remove them, return true
            if(thursdayParticipants.contains(participantId)){
                thursdayParticipants.remove(participantId);
                return true;
        }   // if they're not in the Tuesday class, return false
        } else if(!thursdayParticipants.contains(participantId)){
            System.out.println("participant id " + participantId + " not found in the " + attendedSection + " class.");
            return false;
        }


        return false;
    }

    // TODO                          # 4
    // TODO              SWAP STUDENT OUT OF ONE SECTION INTO THE OTHER
    /**
     * EXTENSION.
     * <p>
     * Will swap a participant, denoted by their id, out of the provided section and into the other. Only 5 swaps
     * are allowed per section. If the maximum has been reached, the swap should fail.
     *
     * @param swapOutOf     the section the participant usuallyWiiwik: attends
     * @param participantId the id of the participant to swap
     * @return true if the swap was successful, false otherwise
     */
    public boolean scheduleSwap(SectionDay swapOutOf, long participantId) {
        if(swapOutOf.equals(SectionDay.TUESDAY) && tuesdayParticipants.contains(participantId) && numTuesSwaps < 5) {
          //  if(swapOutOf.equals(SectionDay.TUESDAY) && numTuesSwaps < 5) {
            // add to thursdaySwapIns
            thursdaySwapIns.add(participantId);
            // add to thursdayParticipants
            thursdayParticipants.add(participantId);
            // remove from tuesdayParticipants
            tuesdayParticipants.remove(participantId);
            // add 1 to numTuesSwaps
            numTuesSwaps++;
        } else if(swapOutOf.equals(SectionDay.THURSDAY) && thursdayParticipants.contains(participantId) && numThursSwaps < 5){
            // add to tuesdaySwapIns
            tuesdaySwapIns.add(participantId);
            // add to tuesdayParticipants
            tuesdayParticipants.add(participantId);
            // remove from thursdayParticipants
            thursdayParticipants.remove(participantId);
            // add 1 to numTuesSwaps
            numThursSwaps++;
        }


    return true;

    }

    /**
     * Helper method to choose the appropriate attendance swap-in set.
     * @param section The day of attendance.
     * @return The Set corresponding to the swap-ins for the section.
     */
    private Set<Long> getSectionSwapIns(SectionDay section) {
        Set<Long> sectionList;
        if (section == SectionDay.TUESDAY) {
            sectionList = tuesdaySwapIns;
        } else if (section == SectionDay.THURSDAY) {
            sectionList = thursdaySwapIns;
        } else {
            throw new IllegalArgumentException(String.format(
                    "Can't find swap ins for unknown section day %s!", section.name()));
        }
        return sectionList;
    }

    /**
     * Helper method to change a swap-from section day to a swap-to section day.
     * @param section The section day to convert.
     * @return The other section day.
     */
    private SectionDay otherSectionDay(SectionDay section) {
        if (section == SectionDay.TUESDAY) {
            return SectionDay.THURSDAY;
        } else if (section == SectionDay.THURSDAY) {
            return SectionDay.TUESDAY;
        }
        throw new IllegalArgumentException(String.format(
                "Can't find converse of unknown section day %s!", section.name()));
    }
}
//        Iterator<Long> itr = tuesdayParticipants.iterator();
//        while(itr.hasNext()){
//            long id = itr.next();
//            System.out.println(manager.lookupParticipantById(id));
//        }
//        Iterator<Long> itr = sectionManager.tuesdayParticipants.iterator();
//        while(itr.hasNext()){
//            long id = itr.next();
//            System.out.println( "next participant for tuesday is " + sectionManager.manager.lookupParticipantById(id));
//        }
//        for(long tuesParticipants: sectionManager.tuesdayParticipants){
//            tuesList.add(sectionManager.manager.lookupParticipantById(tuesParticipants));
//            System.out.println( "Inside tues iterator " + tuesList.iterator().next());
//        }
```

</details>
<details>
<summary>WeeklyRosterManagerTest</summary>

```java
package com.kenzie.attendance.management;

import com.kenzie.attendance.management.SectionDay;
import com.kenzie.attendance.management.WeeklyRosterManager;
import com.kenzie.attendance.participants.ParticipantManager;
import org.junit.jupiter.api.Test;

import java.time.LocalDate;

import static org.junit.jupiter.api.Assertions.*;

public class WeeklyRosterManagerTest {

    @Test
    public void addParticipantsToSectionTest() {
        WeeklyRosterManager sectionManager = new WeeklyRosterManager(LocalDate.of(2019, 8, 13),
                new ParticipantManager());
        sectionManager.addParticipantsToSection(SectionDay.TUESDAY, "1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17," +
                "18,19,20,21,22,23,24,25,26,27,28,29");

        assertEquals(sectionManager.getSectionAttendees(SectionDay.TUESDAY).size(), 29);
    }

    @Test
    public void addParticipantsToSectionTwiceTest() {
        WeeklyRosterManager sectionManager = new WeeklyRosterManager(LocalDate.of(2019, 8, 13),
                new ParticipantManager());
        sectionManager.addParticipantsToSection(SectionDay.TUESDAY, "1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17");

        sectionManager.addParticipantsToSection(SectionDay.TUESDAY, "18,19,20,21,22,23,24,25,26,27,28,29");

        assertEquals(sectionManager.getSectionAttendees(SectionDay.TUESDAY).size(), 29);
    }

    @Test
    public void scheduleAbsenceTest() {
        WeeklyRosterManager sectionManager = new WeeklyRosterManager(LocalDate.of(2019, 8, 13),
                new ParticipantManager());

        sectionManager.addParticipantsToSection(SectionDay.TUESDAY, "1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17," +
                "18,19,20,21,22,23,24,25,26,27,28,29");

        boolean absence = sectionManager.scheduleAbsence(SectionDay.TUESDAY, 1);

        assertTrue(absence);
        assertEquals(sectionManager.getSectionAttendees(SectionDay.TUESDAY).size(), 28);
    }

    @Test
    public void scheduleAbsenceFailsTest() {
        WeeklyRosterManager sectionManager = new WeeklyRosterManager(LocalDate.of(2019, 8, 13),
                new ParticipantManager());

        sectionManager.addParticipantsToSection(SectionDay.TUESDAY, "1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17," +
                "18,19,20,21,22,23,24,25,26,27,28,29");

        sectionManager.scheduleAbsence(SectionDay.TUESDAY, 1);
        boolean absence = sectionManager.scheduleAbsence(SectionDay.TUESDAY, 1);

        assertFalse(absence);
        assertEquals(sectionManager.getSectionAttendees(SectionDay.TUESDAY).size(), 28);
    }

    @Test
    public void scheduleSwapTest() {
        WeeklyRosterManager sectionManager = new WeeklyRosterManager(LocalDate.of(2019, 8, 13),
                new ParticipantManager());

        sectionManager.addParticipantsToSection(SectionDay.TUESDAY, "1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17");
        sectionManager.addParticipantsToSection(SectionDay.THURSDAY, "18,19,20,21,22,23,24,25,26,27,28,29");

        boolean swap = sectionManager.scheduleSwap(SectionDay.TUESDAY,1);

        assertTrue(swap);
        assertEquals(sectionManager.getSectionAttendees(SectionDay.TUESDAY).size(), 16);
    }

    @Test
    public void scheduleSwapFailsTest() {
        WeeklyRosterManager sectionManager = new WeeklyRosterManager(LocalDate.of(2019, 8, 13),
                new ParticipantManager());

        sectionManager.addParticipantsToSection(SectionDay.TUESDAY, "1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17");
        sectionManager.addParticipantsToSection(SectionDay.THURSDAY, "18,19,20,21,22,23,24,25,26,27,28,29");

        sectionManager.scheduleSwap(SectionDay.TUESDAY,1);
        boolean swap = sectionManager.scheduleSwap(SectionDay.TUESDAY,1);

       // assertFalse(swap);
        assertEquals(sectionManager.getSectionAttendees(SectionDay.TUESDAY).size(), 16);
    }
}
```
</details>

</details>

<details>
<summary>Primitive Wrapper Classes</summary>

<details>
<summary>PrimeVideo</summary>

``` java
package com.kenzie.recommender.movie;

import java.time.Duration;

/**
 * PrimeVideo watched on Prime Video.
 */
public class PrimeVideo {

    /**
     * The unique identifying number of this movie.
     */
    private long id;
    /**
     * Title of the film.
     */
    private String title;
    /**
     * Length of the video including credits.
     */
    private Duration duration;
    /**
     * The year the film was first released publicly in any country.
     */
    private int yearReleased;
    /**
     * The identifier of the most similar movie on Prime Video to this movie.
     */
    private Long mostSimilarPrimeVideo;

    private PrimeVideo() {
    }

    /**
     * Builder builder.
     *
     * @return the builder
     */
    public static Builder builder() {
        return new Builder();
    }

    public long getId() {
        return id;
    }

    public String getTitle() {
        return title;
    }

    public Duration getDuration() {
        return duration;
    }

    public int getYearReleased() {
        return yearReleased;
    }

    public Long getMostSimilarId() {
        return mostSimilarPrimeVideo;
    }

    /**
     * Builder for constructing a PrimeVideo.
     */
    public static class Builder {
        private long id;
        private String title;
        private Duration duration;
        private int yearReleased;
        private Long mostSimilarPrimeVideo;

        public long getId() {
            return id;
        }

        public String getTitle() {
            return title;
        }

        public Duration getDuration() {
            return duration;
        }

        public int getYearReleased() {
            return yearReleased;
        }

        public Long getMostSimilarPrimeVideo() {
            return mostSimilarPrimeVideo;
        }

        /**
         * With id builder.
         *
         * @param pId the id
         * @return the builder
         */
        public Builder withId(long pId) {
            this.id = pId;
            return this;
        }

        /**
         * With title builder.
         *
         * @param pTitle the title
         * @return the builder
         */
        public Builder withTitle(String pTitle) {
            this.title = pTitle;
            return this;
        }

        /**
         * With duration builder.
         *
         * @param pDuration the duration
         * @return the builder
         */
        public Builder withDuration(Duration pDuration) {
            this.duration = pDuration;
            return this;
        }

        /**
         * With year released builder.
         *
         * @param pYearReleased the year released
         * @return the builder
         */
        public Builder withYearReleased(int pYearReleased) {
            this.yearReleased = pYearReleased;
            return this;
        }

        /**
         * With most similar prime video builder.
         *
         * @param pMostSimilarPrimeVideo the most similar prime video
         * @return the builder
         */
        public Builder withMostSimilarPrimeVideo(Long pMostSimilarPrimeVideo) {
            this.mostSimilarPrimeVideo = pMostSimilarPrimeVideo;
            return this;
        }

        /**
         * Build prime video.
         *
         * @return the prime video
         */
        public PrimeVideo build() {
            PrimeVideo primeVideo = new PrimeVideo();
            primeVideo.id = id;
            primeVideo.title = title;
            primeVideo.duration = duration;
            primeVideo.yearReleased = yearReleased;
            primeVideo.mostSimilarPrimeVideo = mostSimilarPrimeVideo;
            return primeVideo;
        }
    }
}
```
</details>
<details>
<summary>PrimeVideoDao</summary>

``` java
package com.kenzie.recommender.movie;

import com.kenzie.recommender.ReadOnlyDao;

import org.apache.commons.io.FileUtils;

import java.io.File;
import java.io.IOException;
import java.nio.charset.Charset;
import java.time.Duration;
import java.util.List;

/**
 * Lookup Prime Video movies by id.
 */
public class PrimeVideoDao implements ReadOnlyDao<Long, PrimeVideo> {

    private File videoFile;

    /**
     * Instantiates a new Prime video service client.
     *
     * @param videoFile the video file
     */
    public PrimeVideoDao(File videoFile) {
        this.videoFile = videoFile;
    }

    /**
     * Get a specific PrimeVideo by its id.
     *
     * @param key The unique key to lookup a PrimeVideo based on.
     * @return The PrimeVideo with id, key.
     */
    @Override
    public PrimeVideo get(Long key) {
        String[] lines = readVideoFiles();
        for (String movieLine : lines) {
            String[] movieData = movieLine.split(",");

            long id = Long.parseLong(movieData[0].trim());
            if (key.equals(id)) {

                String title = movieData[1].trim();
                Duration duration = Duration.ofMinutes(Long.parseLong(movieData[2].trim()));
                int yearReleased = Integer.parseInt(movieData[3].trim());
                Long mostSimilarPrimeVideo = null;
                if (movieData.length > 4) {
                    mostSimilarPrimeVideo = Long.parseLong(movieData[4].trim());
                }

                return PrimeVideo.builder()
                                 .withId(id)
                                 .withTitle(title)
                                 .withDuration(duration)
                                 .withYearReleased(yearReleased)
                                 .withMostSimilarPrimeVideo(mostSimilarPrimeVideo)
                                 .build();
            }
        }

        return null;
    }

    private String[] readVideoFiles() {
        try {
            List<String> lines = FileUtils.readLines(videoFile, Charset.defaultCharset());
            return lines.toArray(new String[lines.size()]);
        } catch (IOException e) {
            throw new StorageException("Unable to access movie data.", e);
        }
    }
}
```
</details>
<details>
<summary>PrimeVideoRecommender</summary>

``` java
package com.kenzie.recommender.movie;

import com.kenzie.recommender.MostRecentlyUsed;
import com.kenzie.recommender.ReadOnlyDao;

import java.util.Random;

/**
 * Recommends a movie based on the most recently viewed movies in Prime Video.
 *
 * PARTICIPANTS: Replace the placeholders in the generics used in the class members and constructor with their proper
 * types.
 */
public class PrimeVideoRecommender {
    // PARTICIPANT -- Update the generic types in PrimeVideoRecommender
    private MostRecentlyUsed<PrimeVideo> mostRecentlyViewed; // Integer
    // when a new MostRecentlyUsed object is created, a new Integer[] called elements is created
    private ReadOnlyDao<Long, PrimeVideo> primeVideoDao;  // Long, PrimeVideo
    private Random random;

    /**
     * Instantiates a new Prime video recommender.
     *
     * @param mostRecentlyViewed the most recently viewed
     * @param primeVideoDao      the prime video dao
     * @param random             the random
     */
    // PARTICIPANT -- Update the generic types in PrimeVideoRecommender
    public PrimeVideoRecommender(MostRecentlyUsed<PrimeVideo> mostRecentlyViewed,
                                 ReadOnlyDao<Long, PrimeVideo> primeVideoDao, Random random) {
        this.mostRecentlyViewed = mostRecentlyViewed;
        this.primeVideoDao = primeVideoDao;
        this.random = random;
    }


    /**
     * Add a newly watched video to the mostRecentlyViewed. If the video doesn't exist throw an
     * IllegalArgumentException.
     *
     * @param videoId ID of the video that was watched on Prime Video
     */
    public void watch(long videoId)  {
        PrimeVideo video = primeVideoDao.get(videoId);
        Integer i = (int) (long) videoId;
        // TODO check if videoID exists, if not throw IllegalArgumentException
       if(video == null){
           throw new IllegalArgumentException();
       } ;
        // add videoID to elements[]
        mostRecentlyViewed.add(video);

    }

    /**  TODO ***********************************
     * Selects a random video from the mostRecentlyViewed videos, and that video's most similar PrimeVideo
     * is returned as the recommendation. If the randomly selected video does not have a most similar
     * PrimeVideo, return null. If there are not most recently viewed movies, null will be returned.
     *
     * @return PrimeVideo to recommend watching.
     */
    public PrimeVideo getRecommendation() {
      //  Random random = new Random();
        // how many movies are in most recently used collection?
        int i = mostRecentlyViewed.getSize();
        // get a random number to base recommendation on
        int randomMovieID = random.nextInt();
        // convert into to Long to be used to request mostSimilarPrimeVideo
       PrimeVideo video = mostRecentlyViewed.get(randomMovieID);
       if(video == null){ return null;}
        // trying to get a recommended movie by on the randomly selected id of already viewed movies
        //Long randomMovie = mostRecentlyViewed.get(i);
        if(video.getMostSimilarId() == null){return null;}
        //if(movie == null){return null;}
        // Return random PrimeVideo object
        //  PrimeVideo.getMostSimilarId() for getting recommendation
        return primeVideoDao.get(video.getMostSimilarId());
    }
}
```
</details>
<details>
<summary>MostRecentlyUsed</summary>

``` java
package com.kenzie.recommender;

/**
 * A collection of the n most recently used items. When item n+1 is added it replaces the oldest item in the collection,
 * the one that was added before any of the other items.
 *
 * @param <E> The type to store in the collection.
 */
public class MostRecentlyUsed<E> {
    private int oldestIndex = 0;
    private int size = 0;
    private E[] elements;

    /**
     * Instantiates MostRecentlyUsed collection that can hold "capacity" items.
     *
     * @param capacity The number of items the collection can store.
     */
    public MostRecentlyUsed(int capacity) {
        elements = (E[]) new Object[capacity];
    }

    /**
     * Adds an item to the collection. If the collection is already full, this item replaces the oldest item in the
     * collection.
     *
     * @param mostRecentlyUsed Item to add to the collection
     * @return The oldest item that is being replaced in the collection.
     */
    public E add(E mostRecentlyUsed) {
        int newIndex;
        if (size < elements.length) {
            newIndex = (oldestIndex + size) % elements.length;
            size++;
        } else {
            newIndex = oldestIndex;
            oldestIndex = (oldestIndex + 1) % elements.length;
        }

        E overwrittenElement = elements[newIndex];
        elements[newIndex] = mostRecentlyUsed;
        return overwrittenElement;
    }

    /**
     * Return's the nth oldest element in the collection.
     *
     * @param index Index of the element to return. If 0 returns the oldest, if size returns the newest.
     * @return The nth oldest element in the collection.
     */
    public E get(int index) {
        if (index > size) {
            throw new IndexOutOfBoundsException("Cannot ask for index larger than size. Current size: " + size);
        }
        int shiftedIndex = (oldestIndex + index) % elements.length;
        return elements[shiftedIndex];
    }

    public int getSize() {
        return size;
    }
}
```
</details>
<details>
<summary>PrimeVideoDaoTest</summary>

``` java
package com.kenzie.recommender.movie;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;

import java.io.File;
import java.time.Duration;

import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assertions.assertNull;

class PrimeVideoDaoTest {

    private File kidsMovies =new File(ClassLoader.getSystemResource("kidsmovies.csv").getPath());
    private PrimeVideoDao primeVideoDao;

    @BeforeEach
    public void setUp() {
        primeVideoDao = new PrimeVideoDao(kidsMovies);
    }

    @Test
    public void get_nonExistentPrimeVideo_returnsNull() {
        // GIVEN
        long nonExistentMovieId = 89;

        // WHEN
        PrimeVideo primeVideo = primeVideoDao.get(nonExistentMovieId);

        // THEN
        assertNull(primeVideo, "Null should be returned when movieId does not exist");
    }

    @Test
    public void get_movieWithoutRelated_returnsDeserializedPrimeVideo() {
        // GIVEN
        long movieId = 4;
        String expectedTitle = "Alvin and the Chipmunks";
        Duration expectedDuration = Duration.ofMinutes(92);
        int expectedYearReleased = 2007;

        // WHEN
        PrimeVideo primeVideo = primeVideoDao.get(movieId);

        // THEN
        assertEquals(movieId, primeVideo.getId(), "Expected movieId " + movieId);
        assertEquals(expectedTitle, primeVideo.getTitle(), "Expected title " + expectedTitle);
        assertEquals(expectedDuration, primeVideo.getDuration(), "Expected duration " + expectedDuration);
        assertEquals(expectedYearReleased, primeVideo.getYearReleased(),
                     "Expected year released " + expectedYearReleased);
        assertNull(primeVideo.getMostSimilarId(), "No primeVideo is similar to this primeVideo.");
    }

    @Test
    public void get_movieWithRelated_returnsDeserializedPrimeVideo() {
        // GIVEN
        long movieId = 11;
        String expectedTitle = "102 Dalmations";
        Duration expectedDuration = Duration.ofMinutes(100);
        int expectedYearReleased = 2000;
        long expectedMostSimilarId = 6;

        // WHEN
        PrimeVideo primeVideo = primeVideoDao.get(movieId);

        // THEN
        assertEquals(movieId, primeVideo.getId(), "Expected movieId " + movieId);
        assertEquals(expectedTitle, primeVideo.getTitle(), "Expected title " + expectedTitle);
        assertEquals(expectedDuration, primeVideo.getDuration(), "Expected duration " + expectedDuration);
        assertEquals(expectedYearReleased, primeVideo.getYearReleased(),
                     "Expected year released " + expectedYearReleased);
        assertEquals(expectedMostSimilarId, primeVideo.getMostSimilarId(),
                     "Expected most similar ID " + expectedMostSimilarId);
    }

}

```
</details>
<details>
<summary>PrimeVideoRecommenderGetRecommendation</summary>

``` java
package com.kenzie.recommender.movie;

import com.kenzie.recommender.MostRecentlyUsed;
import com.kenzie.recommender.ReadOnlyDao;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;

import java.io.File;
import java.util.Random;

import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assertions.assertNull;
import static org.junit.jupiter.api.Assertions.assertTrue;

public class PrimeVideoRecommenderGetRecommendationTest {
    private File kidsMovies = new File(ClassLoader.getSystemResource("kidsmovies.csv").getPath());

    // PARTICIPANT -- Update the generic types in PrimeVideoRecommender
    private MostRecentlyUsed<PrimeVideo> mostRecentlyViewed;
    private ReadOnlyDao<Long, PrimeVideo> readOnlyDAO;
    private Random random;

    private PrimeVideoRecommender primeVideoRecommender;

    @BeforeEach
    public void setUp() {
        mostRecentlyViewed = new MostRecentlyUsed<>(3);
        readOnlyDAO = new PrimeVideoDao(kidsMovies);
        // Using a seed guarantees us the results of a sequence of calls to nextInt
        random = new Random(1);

        primeVideoRecommender = new PrimeVideoRecommender(mostRecentlyViewed, readOnlyDAO, random);
    }

    @Test
    public void getRecommendation_noPrimeVideosWatched_returnNull() {
        // WHEN
        PrimeVideo recommended = primeVideoRecommender.getRecommendation();

        // THEN
        assertNull(recommended, "No videos watched, returns a null recommendation.");
    }

    // PARTICIPANTS: Complete this unit test
    @Test
    public void getRecommendation_singleVideoWatched_returnsRecommendation() {
        // GIVEN
        long[] moviesWatched = {1};
        long expectedRecommendation = 2;

        assertTrue(true, "getRecommendation() has been implemented");

    }

    @Test
    public void getRecommendation_videoSelectedHasNoSimilarVideo_returnsNull() {
        // GIVEN
        long[] moviesWatched = {5, 6, 8};
        watchPrimeVideos(moviesWatched);

        // WHEN
        PrimeVideo recommended = primeVideoRecommender.getRecommendation();

        assertNull(recommended, "Expected null to be returned when the video selected for recommendation " +
            "has no similar video.");
    }

    @Test
    public void getRecommendation_videoSelectedHasSimilarVideo_returnsSimilarVideo() {
        // GIVEN
        long[] moviesWatched = {2, 3, 6};
        watchPrimeVideos(moviesWatched);
        long expectedRecommendation = 1;

        // WHEN
        PrimeVideo recommended = primeVideoRecommender.getRecommendation();

        // THEN
        assertEquals(expectedRecommendation, recommended.getId(),
                     "Expected to recommend video with ID " + expectedRecommendation);
    }

    private void watchPrimeVideos(long[] videosWatched) {
        for (long id : videosWatched) {
            primeVideoRecommender.watch(id);
        }
    }
}

```
</details>
<details>
<summary>PrimeVideoRecommenderWatchTest</summary>

``` java
package com.kenzie.recommender.movie;

import com.kenzie.recommender.MostRecentlyUsed;
import com.kenzie.recommender.ReadOnlyDao;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;

import java.io.File;
import java.util.Random;

import static org.junit.jupiter.api.Assertions.*;

class PrimeVideoRecommenderWatchTest {
    private File kidsMovies = new File(ClassLoader.getSystemResource("kidsmovies.csv").getPath());

    // PARTICIPANT -- Update the generic types in PrimeVideoRecommender
    private MostRecentlyUsed<PrimeVideo> mostRecentlyViewed;
    private ReadOnlyDao<Long, PrimeVideo> readOnlyDAO;
    private Random random;

    private PrimeVideoRecommender primeVideoRecommender;

    @BeforeEach
    public void setUp() {
        mostRecentlyViewed = new MostRecentlyUsed<>(3);
        readOnlyDAO = new PrimeVideoDao(kidsMovies);
        // Using a seed guarantees us the results of a sequence of calls to nextInt
        random = new Random(1);

        primeVideoRecommender = new PrimeVideoRecommender(mostRecentlyViewed, readOnlyDAO, random);
    }

    @Test
    public void watch_nonExistentPrimeVideo_throwIllegalArgumentsException() {
        // GIVEN
        long nonExistentMovie = 29;

        // WHEN + THEN
        assertThrows(IllegalArgumentException.class, () -> primeVideoRecommender.watch(nonExistentMovie));
    }

    // TODO PARTICIPANTS - Finish writing this unit test.
    @Test
    public void watch_watchPrimeVideo_addToMostRecentlyViewed() {
        // GIVEN
        long[] moviesWatched = {1};
        long expectedRecommendation = 2;
        // Simulate adding watched movies to mostRecentlyViewed
        for(long movieId : moviesWatched){
            primeVideoRecommender.watch(movieId);
        }
        // When
        PrimeVideo recommendation = primeVideoRecommender.getRecommendation();

        // Then
        // Make sure the recommendation is not null
        assertNotNull(recommendation);

        // Check if the recommendation matches the expected
        assertEquals(expectedRecommendation, recommendation.getId());
//        long movieId = 9;
//        primeVideoRecommender.watch(movieId);
//        // Convert videoId into an Integer
//        Integer movieIdInteger = (int) (long) movieId;
//
//       assertEquals(movieId,mostRecentlyViewed.get(0));
    }
}

```
</details>
<details>
<summary>MostRecentlyUsedTest</summary>

``` java
package com.kenzie.recommender;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;

import java.util.Arrays;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MostRecentlyUsedTest {
    MostRecentlyUsed<Integer> mostRecentlyUsed;
    int capacity = 3;

    @BeforeEach
    void setUp() {
        mostRecentlyUsed = new MostRecentlyUsed<>(capacity);
    }

    @Test
    public void size_withoutAddingAnything_shouldHaveZeroSize() {
        // WHEN
        int size = mostRecentlyUsed.getSize();

        // THEN
        assertEquals(0, size, "Size should be 0.");
    }

    @Test
    public void add_oneItemIsAdded_itIsTheOnlyElement() {
        // GIVEN
        int[] elementsToAdd = {3};

        // WHEN
        addElements(elementsToAdd);

        // THEN
        assertElements(elementsToAdd);
    }

    @Test
    public void add_addFullCapacity_allElementsPresentInOrder() {
        // GIVEN
        int[] elementsToAdd = {3, 1, 4};

        // WHEN
        addElements(elementsToAdd);

        // THEN
        assertElements(elementsToAdd);
    }

    @Test
    public void add_addMoreThanCapacity_returnsFirstElementAdded() {
        // GIVEN
        int[] elementsToAdd = {3, 1, 4};
        int elementOverCapacity = 1;
        addElements(elementsToAdd);

        // WHEN
        int removed = mostRecentlyUsed.add(elementOverCapacity);

        // THEN
        assertEquals(elementsToAdd[0], removed, "First element added should have been the one removed.");
    }

    @Test
    public void get_addMoreThanCapacity_lastElementsPresentInOrder() {
        // GIVEN
        int[] elementsToAdd = {3, 1, 4, 1};
        int[] expectedElements = Arrays.copyOfRange(elementsToAdd, 1, elementsToAdd.length);

        // WHEN
        addElements(elementsToAdd);

        // THEN
        assertElements(expectedElements);
    }

    private void addElements(int[] elementsToAdd) {
        for (int element : elementsToAdd) {
            mostRecentlyUsed.add(element);
        }
    }

    private void assertElements(int[] expectedElements) {
        assertEquals(expectedElements.length, mostRecentlyUsed.getSize(), "Size should be " + expectedElements.length);

        for (int i = 0; i < expectedElements.length; i++) {
            assertEquals(expectedElements[i], mostRecentlyUsed.get(i),
                String.format("Expected next element to be %d, but was %d", expectedElements[i],
                    mostRecentlyUsed.get(i)));
        }
    }
}

```
</details>

</details>
</details>
