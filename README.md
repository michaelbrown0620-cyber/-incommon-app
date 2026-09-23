import React, { useMemo, useState } from "react";

/*
  INCOMMON
  Social discovery through shared interests.

  Starter MVP
  - Onboarding
  - Large hobby/interest library
  - Profile
  - Discover people
  - Common Ground matching
  - Communities
  - Connections
  - Basic messaging
  - Local demo state
*/

type Tab =
  | "home"
  | "discover"
  | "communities"
  | "connections"
  | "messages"
  | "profile";

type User = {
  id: string;
  name: string;
  username: string;
  location: string;
  bio: string;
  avatar: string;
  interests: string[];
};

type Community = {
  id: string;
  name: string;
  description: string;
  category: string;
  members: number;
  interests: string[];
};

const INTEREST_CATEGORIES: Record<string, string[]> = {
  "Sports & Fitness": [
    "Football",
    "Basketball",
    "Baseball",
    "Soccer",
    "Hockey",
    "Tennis",
    "Golf",
    "Running",
    "Walking",
    "Hiking",
    "Cycling",
    "Swimming",
    "Weightlifting",
    "Yoga",
    "Martial Arts",
    "Bowling",
    "Pickleball",
    "Fishing",
    "Camping",
    "Rock Climbing",
  ],

  Creative: [
    "Drawing",
    "Painting",
    "Photography",
    "Filmmaking",
    "Writing",
    "Poetry",
    "Music",
    "Singing",
    "Guitar",
    "Piano",
    "Drums",
    "Acting",
    "Theater",
    "Dancing",
    "Graphic Design",
    "Digital Art",
    "Crafts",
    "Pottery",
    "Woodworking",
    "Sewing",
    "Knitting",
    "Jewelry Making",
  ],

  Technology: [
    "Programming",
    "Artificial Intelligence",
    "Robotics",
    "Electronics",
    "3D Printing",
    "Computers",
    "Game Development",
    "Cybersecurity",
    "Virtual Reality",
    "Drones",
    "Tech Projects",
  ],

  Gaming: [
    "Console Gaming",
    "PC Gaming",
    "Mobile Gaming",
    "Retro Gaming",
    "Board Games",
    "Card Games",
    "Tabletop RPGs",
    "Dungeons & Dragons",
    "Strategy Games",
    "Simulation Games",
    "Racing Games",
    "Fighting Games",
    "Esports",
  ],

  "Outdoors & Adventure": [
    "Hiking",
    "Fishing",
    "Hunting",
    "Gardening",
    "Bird Watching",
    "Nature Photography",
    "Travel",
    "Road Trips",
    "Backpacking",
    "Kayaking",
    "Boating",
    "Geocaching",
    "Stargazing",
    "Off-Roading",
  ],

  "Cars & Motorsports": [
    "Cars",
    "Car Restoration",
    "Car Modification",
    "Classic Cars",
    "Muscle Cars",
    "Imports",
    "Trucks",
    "Motorcycles",
    "Motorsports",
    "Racing",
    "Track Days",
    "Automotive Photography",
    "Car Detailing",
  ],

  "Food & Cooking": [
    "Cooking",
    "Baking",
    "Grilling",
    "BBQ",
    "Meal Prep",
    "International Cuisine",
    "Coffee",
    "Tea",
    "Food Photography",
    "Restaurant Exploration",
  ],

  Collecting: [
    "Coins",
    "Stamps",
    "Sports Cards",
    "Trading Cards",
    "Comics",
    "Toys",
    "Sneakers",
    "Watches",
    "Vinyl Records",
    "Books",
    "Antiques",
    "Memorabilia",
  ],

  Learning: [
    "History",
    "Science",
    "Astronomy",
    "Psychology",
    "Languages",
    "Mathematics",
    "Philosophy",
    "Business",
    "Personal Finance",
    "Public Speaking",
    "Skill Development",
  ],

  Lifestyle: [
    "Reading",
    "Personal Development",
    "Meditation",
    "Journaling",
    "Fashion",
    "Interior Design",
    "DIY",
    "Home Improvement",
  ],

  "Animals & Pets": [
    "Dogs",
    "Cats",
    "Aquariums",
    "Reptiles",
    "Birds",
    "Horseback Riding",
    "Animal Rescue",
    "Pet Training",
  ],

  "Travel & Culture": [
    "Traveling",
    "Road Trips",
    "International Travel",
    "Local Exploration",
    "Museums",
    "Languages",
    "Cultural Experiences",
  ],

  Entertainment: [
    "Movies",
    "Television",
    "Anime",
    "Comics",
    "Music",
    "Concerts",
    "Podcasts",
    "Stand-Up Comedy",
    "Theater",
  ],

  Community: [
    "Volunteering",
    "Charity",
    "Local Events",
    "Meetups",
    "Group Activities",
    "Community Projects",
    "Skill Sharing",
  ],
};

const ALL_INTERESTS = Object.values(INTEREST_CATEGORIES).flat();

const INITIAL_USER: User = {
  id: "me",
  name: "Your Name",
  username: "@yourname",
  location: "Your Area",
  bio: "I'm here to meet people, discover new interests, and find my people.",
  avatar: "🙂",
  interests: [
    "Cars",
    "Photography",
    "Music",
    "Travel",
    "Gaming",
  ],
};

const DEMO_USERS: User[] = [
  {
    id: "alex",
    name: "Alex",
    username: "@alex",
    location: "Atlanta Area",
    bio: "Photography, cars, hiking and finding new places to explore.",
    avatar: "📸",
    interests: [
      "Photography",
      "Cars",
      "Hiking",
      "Travel",
      "Music",
      "Gaming",
      "Movies",
      "Cooking",
    ],
  },
  {
    id: "jordan",
    name: "Jordan",
    username: "@jordan",
    location: "Atlanta Area",
    bio: "Huge sports fan who loves gaming and meeting new people.",
    avatar: "🏀",
    interests: [
      "Basketball",
      "Football",
      "Gaming",
      "Music",
      "Movies",
      "Travel",
      "Cooking",
    ],
  },
  {
    id: "sam",
    name: "Sam",
    username: "@sam",
    location: "Atlanta Area",
    bio: "Creative person who loves art, photography and live music.",
    avatar: "🎨",
    interests: [
      "Photography",
      "Drawing",
      "Painting",
      "Music",
      "Concerts",
      "Travel",
      "Writing",
    ],
  },
  {
    id: "taylor",
    name: "Taylor",
    username: "@taylor",
    location: "Atlanta Area",
    bio: "Always looking for the next adventure.",
    avatar: "🥾",
    interests: [
      "Hiking",
      "Camping",
      "Fishing",
      "Travel",
      "Road Trips",
      "Photography",
      "Cars",
    ],
  },
  {
    id: "chris",
    name: "Chris",
    username: "@chris",
    location: "Atlanta Area",
    bio: "Tech enthusiast, gamer and hobby collector.",
    avatar: "💻",
    interests: [
      "Programming",
      "Artificial Intelligence",
      "PC Gaming",
      "Retro Gaming",
      "3D Printing",
      "Robotics",
      "Technology",
    ],
  },
];

const COMMUNITIES: Community[] = [
  {
    id: "cars",
    name: "Car Enthusiasts",
    description: "Talk cars, modifications, restoration, racing and everything automotive.",
    category: "Cars & Motorsports",
    members: 284,
    interests: ["Cars", "Racing", "Car Modification"],
  },
  {
    id: "photography",
    name: "Photography",
    description: "Share photos, learn new techniques and meet other photographers.",
    category: "Creative",
    members: 421,
    interests: ["Photography", "Nature Photography", "Automotive Photography"],
  },
  {
    id: "hiking",
    name: "Weekend Hikers",
    description: "Find people who enjoy getting outside and exploring.",
    category: "Outdoors & Adventure",
    members: 197,
    interests: ["Hiking", "Camping", "Travel"],
  },
  {
    id: "gaming",
    name: "Gamers",
    description: "Console, PC, mobile, retro and tabletop gaming.",
    category: "Gaming",
    members: 612,
    interests: ["Gaming", "PC Gaming", "Console Gaming"],
  },
];

function getSharedInterests(a: User, b: User) {
  return a.interests.filter((interest) => b.interests.includes(interest));
}

function getCommonScore(a: User, b: User) {
  const shared = getSharedInterests(a, b);

  if (shared.length === 0) return 0;

  return Math.min(
    99,
    Math.round((shared.length / Math.max(a.interests.length, b.interests.length)) * 100)
  );
}

function Icon({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <span className="text-lg leading-none" aria-hidden="true">
      {children}
    </span>
  );
}

function Pill({
  children,
  selected = false,
  onClick,
}: {
  children: React.ReactNode;
  selected?: boolean;
  onClick?: () => void;
}) {
  return (
    <button
      onClick={onClick}
      className={`rounded-full border px-3 py-1.5 text-sm transition ${
        selected
          ? "border-indigo-600 bg-indigo-600 text-white"
          : "border-zinc-200 bg-white text-zinc-700 hover:border-indigo-400 hover:bg-indigo-50"
      }`}
    >
      {children}
    </button>
  );
}

function UserCard({
  user,
  currentUser,
  onView,
  connected,
  onConnect,
}: {
  user: User;
  currentUser: User;
  onView: () => void;
  connected: boolean;
  onConnect: () => void;
}) {
  const shared = getSharedInterests(currentUser, user);
  const score = getCommonScore(currentUser, user);

  return (
    <div className="rounded-2xl border border-zinc-200 bg-white p-5 shadow-sm">
      <div className="flex items-start gap-4">
        <div className="flex h-14 w-14 shrink-0 items-center justify-center rounded-full bg-indigo-100 text-2xl">
          {user.avatar}
        </div>

        <div className="min-w-0 flex-1">
          <div className="flex items-center justify-between gap-2">
            <div>
              <h3 className="font-bold text-zinc-900">{user.name}</h3>
              <p className="text-sm text-zinc-500">{user.username}</p>
            </div>

            <div className="rounded-full bg-indigo-50 px-3 py-1 text-sm font-bold text-indigo-700">
              {score}% common
            </div>
          </div>

          <p className="mt-2 text-sm text-zinc-600">{user.bio}</p>

          <p className="mt-3 text-xs font-semibold uppercase tracking-wide text-indigo-600">
            {shared.length} things in common
          </p>

          <div className="mt-2 flex flex-wrap gap-2">
            {shared.slice(0, 5).map((interest) => (
              <span
                key={interest}
                className="rounded-full bg-zinc-100 px-2.5 py-1 text-xs text-zinc-700"
              >
                {interest}
              </span>
            ))}
          </div>

          <div className="mt-4 flex gap-2">
            <button
              onClick={onView}
              className="rounded-xl border border-zinc-200 px-4 py-2 text-sm font-semibold hover:bg-zinc-50"
            >
              View Profile
            </button>

            <button
              onClick={onConnect}
              className={`rounded-xl px-4 py-2 text-sm font-semibold ${
                connected
                  ? "bg-zinc-100 text-zinc-500"
                  : "bg-indigo-600 text-white hover:bg-indigo-700"
              }`}
            >
              {connected ? "Connected" : "Connect"}
            </button>
          </div>
        </div>
      </div>
    </div>
  );
}

export default function App() {
  const [user, setUser] = useState<User>(INITIAL_USER);
  const [activeTab, setActiveTab] = useState<Tab>("home");

  const [onboarding, setOnboarding] = useState(true);
  const [onboardingStep, setOnboardingStep] = useState(0);

  const [selectedCategory, setSelectedCategory] =
    useState("Sports & Fitness");

  const [interestSearch, setInterestSearch] = useState("");

  const [connectedIds, setConnectedIds] = useState<string[]>([]);

  const [selectedUser, setSelectedUser] = useState<User | null>(null);

  const [messageUser, setMessageUser] = useState<User | null>(null);

  const [messageText, setMessageText] = useState("");

  const [messages, setMessages] = useState<
    Record<string, { from: string; text: string }[]>
  >({});

  const [joinedCommunities, setJoinedCommunities] = useState<string[]>([]);

  const [profileName, setProfileName] = useState(user.name);
  const [profileBio, setProfileBio] = useState(user.bio);

  const filteredInterests = useMemo(() => {
    if (!interestSearch.trim()) {
      return INTEREST_CATEGORIES[selectedCategory] || [];
    }

    const search = interestSearch.toLowerCase();

    return ALL_INTERESTS.filter((interest) =>
      interest.toLowerCase().includes(search)
    );
  }, [interestSearch, selectedCategory]);

  const discoverUsers = useMemo(() => {
    return [...DEMO_USERS].sort(
      (a, b) => getCommonScore(user, b) - getCommonScore(user, a)
    );
  }, [user]);

  function toggleInterest(interest: string) {
    setUser((previous) => {
      const exists = previous.interests.includes(interest);

      return {
        ...previous,
        interests: exists
          ? previous.interests.filter((item) => item !== interest)
          : [...previous.interests, interest],
      };
    });
  }

  function connectToUser(userId: string) {
    setConnectedIds((previous) =>
      previous.includes(userId)
        ? previous
        : [...previous, userId]
    );
  }

  function sendMessage() {
    if (!messageUser || !messageText.trim()) return;

    setMessages((previous) => ({
      ...previous,
      [messageUser.id]: [
        ...(previous[messageUser.id] || []),
        {
          from: "me",
          text: messageText.trim(),
        },
      ],
    }));

    setMessageText("");
  }

  function saveProfile() {
    setUser((previous) => ({
      ...previous,
      name: profileName,
      bio: profileBio,
    }));

    alert("Profile saved!");
  }

  function finishOnboarding() {
    setOnboarding(false);
    setActiveTab("home");
  }

  if (onboarding) {
    return (
      <div className="min-h-screen bg-gradient-to-br from-indigo-50 via-white to-purple-50">
        <div className="mx-auto flex min-h-screen max-w-4xl items-center justify-center p-6">
          <div className="w-full max-w-2xl rounded-3xl border border-zinc-200 bg-white p-8 shadow-xl md:p-12">

            {onboardingStep === 0 && (
              <div className="text-center">
                <div className="mx-auto flex h-20 w-20 items-center justify-center rounded-3xl bg-indigo-600 text-3xl text-white shadow-lg">
                  ◎
                </div>

                <h1 className="mt-6 text-4xl font-black tracking-tight text-zinc-900">
                  InCommon
                </h1>

                <p className="mx-auto mt-4 max-w-lg text-lg text-zinc-600">
                  Find your people through what you have in common.
                </p>

                <p className="mx-auto mt-3 max-w-md text-sm text-zinc-500">
                  Discover people, hobbies and communities built around the
                  things you enjoy.
                </p>

                <button
                  onClick={() => setOnboardingStep(1)}
                  className="mt-8 rounded-2xl bg-indigo-600 px-8 py-4 font-bold text-white shadow-lg hover:bg-indigo-700"
                >
                  Get Started
                </button>
              </div>
            )}

            {onboardingStep === 1 && (
              <div>
                <div className="text-center">
                  <h2 className="text-3xl font-black text-zinc-900">
                    What should we call you?
                  </h2>

                  <p className="mt-2 text-zinc-500">
                    You can change this later.
                  </p>
                </div>

                <input
                  value={profileName}
                  onChange={(event) => setProfileName(event.target.value)}
                  placeholder="Your name"
                  className="mt-8 w-full rounded-2xl border border-zinc-200 px-4 py-4 outline-none focus:border-indigo-500 focus:ring-2 focus:ring-indigo-100"
                />

                <button
                  onClick={() => {
                    setUser((previous) => ({
                      ...previous,
                      name: profileName || "Your Name",
                    }));
                    setOnboardingStep(2);
                  }}
                  className="mt-5 w-full rounded-2xl bg-indigo-600 py-4 font-bold text-white"
                >
                  Continue
                </button>
              </div>
            )}

            {onboardingStep === 2 && (
              <div>
                <div className="text-center">
                  <h2 className="text-3xl font-black text-zinc-900">
                    What are you into?
                  </h2>

                  <p className="mt-2 text-zinc-500">
                    Pick as many interests as you want.
                  </p>
                </div>

                <div className="mt-6">
                  <input
                    value={interestSearch}
                    onChange={(event) =>
                      setInterestSearch(event.target.value)
                    }
                    placeholder="Search hobbies and interests..."
                    className="w-full rounded-2xl border border-zinc-200 px-4 py-3 outline-none focus:border-indigo-500"
                  />
                </div>

                {!interestSearch && (
                  <div className="mt-5 flex gap-2 overflow-x-auto pb-2">
                    {Object.keys(INTEREST_CATEGORIES).map((category) => (
                      <button
                        key={category}
                        onClick={() => setSelectedCategory(category)}
                        className={`whitespace-nowrap rounded-full px-4 py-2 text-sm font-semibold ${
                          selectedCategory === category
                            ? "bg-indigo-600 text-white"
                            : "bg-zinc-100 text-zinc-700"
                        }`}
                      >
                        {category}
                      </button>
                    ))}
                  </div>
                )}

                <div className="mt-5 flex max-h-72 flex-wrap gap-2 overflow-y-auto">
                  {filteredInterests.map((interest) => (
                    <Pill
                      key={interest}
                      selected={user.interests.includes(interest)}
                      onClick={() => toggleInterest(interest)}
                    >
                      {interest}
                    </Pill>
                  ))}
                </div>

                <p className="mt-4 text-center text-sm text-zinc-500">
                  {user.interests.length} interests selected
                </p>

                <button
                  onClick={() => setOnboardingStep(3)}
                  disabled={user.interests.length === 0}
                  className="mt-5 w-full rounded-2xl bg-indigo-600 py-4 font-bold text-white disabled:cursor-not-allowed disabled:opacity-40"
                >
                  Continue
                </button>
              </div>
            )}

            {onboardingStep === 3 && (
              <div className="text-center">
                <div className="mx-auto flex h-20 w-20 items-center justify-center rounded-full bg-green-100 text-4xl">
                  ✓
                </div>

                <h2 className="mt-6 text-3xl font-black text-zinc-900">
                  You're ready.
                </h2>

                <p className="mx-auto mt-3 max-w-md text-zinc-600">
                  InCommon will use your interests to help you discover people
                  and communities you may have something in common with.
                </p>

                <div className="mt-6 flex flex-wrap justify-center gap-2">
                  {user.interests.slice(0, 12).map((interest) => (
                    <Pill key={interest}>{interest}</Pill>
                  ))}
                </div>

                <button
                  onClick={finishOnboarding}
                  className="mt-8 rounded-2xl bg-indigo-600 px-8 py-4 font-bold text-white"
                >
                  Enter InCommon
                </button>
              </div>
            )}
          </div>
        </div>
      </div>
    );
  }

  return (
    <div className="min-h-screen bg-zinc-50 text-zinc-900">

      {/* HEADER */}
      <header className="sticky top-0 z-40 border-b border-zinc-200 bg-white/95 backdrop-blur">
        <div className="mx-auto flex max-w-7xl items-center justify-between px-4 py-3 md:px-6">
          <button
            onClick={() => {
              setActiveTab("home");
              setSelectedUser(null);
            }}
            className="flex items-center gap-3"
          >
            <div className="flex h-10 w-10 items-center justify-center rounded-xl bg-indigo-600 text-xl font-black text-white">
              ◎
            </div>

            <div className="text-left">
              <div className="font-black leading-none">InCommon</div>
              <div className="hidden text-xs text-zinc-500 sm:block">
                Find your people
              </div>
            </div>
          </button>

          <div className="hidden items-center gap-1 md:flex">
            <NavButton
              active={activeTab === "home"}
              onClick={() => setActiveTab("home")}
            >
              <Icon>⌂</Icon> Home
            </NavButton>

            <NavButton
              active={activeTab === "discover"}
              onClick={() => setActiveTab("discover")}
            >
              <Icon>⌕</Icon> Discover
            </NavButton>

            <NavButton
              active={activeTab === "communities"}
              onClick={() => setActiveTab("communities")}
            >
              <Icon>◉</Icon> Communities
            </NavButton>

            <NavButton
              active={activeTab === "connections"}
              onClick={() => setActiveTab("connections")}
            >
              <Icon>♧</Icon> Connections
            </NavButton>

            <NavButton
              active={activeTab === "messages"}
              onClick={() => setActiveTab("messages")}
            >
              <Icon>✉</Icon> Messages
            </NavButton>

            <NavButton
              active={activeTab === "profile"}
              onClick={() => setActiveTab("profile")}
            >
              <Icon>●</Icon> Profile
            </NavButton>
          </div>

          <button
            onClick={() => setActiveTab("profile")}
            className="flex h-10 w-10 items-center justify-center rounded-full bg-indigo-100 text-lg"
          >
            {user.avatar}
          </button>
        </div>

        {/* MOBILE NAV */}
        <div className="flex overflow-x-auto border-t border-zinc-100 px-2 py-2 md:hidden">
          {[
            ["home", "⌂", "Home"],
            ["discover", "⌕", "Discover"],
            ["communities", "◉", "Groups"],
            ["connections", "♧", "People"],
            ["messages", "✉", "Messages"],
            ["profile", "●", "Profile"],
          ].map(([tab, icon, label]) => (
            <button
              key={tab}
              onClick={() => setActiveTab(tab as Tab)}
              className={`min-w-[80px] rounded-xl px-3 py-2 text-xs font-semibold ${
                activeTab === tab
                  ? "bg-indigo-50 text-indigo-700"
                  : "text-zinc-500"
              }`}
            >
              <div className="text-base">{icon}</div>
              {label}
            </button>
          ))}
        </div>
      </header>

      {/* CONTENT */}
      <main className="mx-auto max-w-7xl px-4 py-6 md:px-6 md:py-10">

        {selectedUser ? (
          <PersonProfile
            user={selectedUser}
            currentUser={user}
            connected={connectedIds.includes(selectedUser.id)}
            onBack={() => setSelectedUser(null)}
            onConnect={() => connectToUser(selectedUser.id)}
            onMessage={() => setMessageUser(selectedUser)}
          />
        ) : activeTab === "home" ? (
          <HomeScreen
            user={user}
            discoverUsers={discoverUsers}
            connectedIds={connectedIds}
            onDiscover={() => setActiveTab("discover")}
            onViewUser={(person) => setSelectedUser(person)}
            onConnect={connectToUser}
          />
        ) : activeTab === "discover" ? (
          <DiscoverScreen
            user={user}
            users={discoverUsers}
            connectedIds={connectedIds}
            onViewUser={(person) => setSelectedUser(person)}
            onConnect={connectToUser}
          />
        ) : activeTab === "communities" ? (
          <CommunitiesScreen
            communities={COMMUNITIES}
            joinedCommunities={joinedCommunities}
            onToggleJoin={(id) =>
              setJoinedCommunities((previous) =>
                previous.includes(id)
                  ? previous.filter((item) => item !== id)
                  : [...previous, id]
              )
            }
          />
        ) : activeTab === "connections" ? (
          <ConnectionsScreen
            users={DEMO_USERS.filter((person) =>
              connectedIds.includes(person.id)
            )}
            onViewUser={(person) => setSelectedUser(person)}
            onMessage={(person) => setMessageUser(person)}
          />
        ) : activeTab === "messages" ? (
          <MessagesScreen
            users={DEMO_USERS.filter((person) =>
              connectedIds.includes(person.id)
            )}
            selectedUser={messageUser}
            messages={messages}
            messageText={messageText}
            setMessageText={setMessageText}
            onSelectUser={setMessageUser}
            onSend={sendMessage}
          />
        ) : (
          <ProfileScreen
            user={user}
            profileName={profileName}
            profileBio={profileBio}
            setProfileName={setProfileName}
            setProfileBio={setProfileBio}
            interests={user.interests}
            onToggleInterest={toggleInterest}
            onSave={saveProfile}
          />
        )}

      </main>
    </div>
  );
}

/* -------------------------------------------------------
   NAVIGATION
------------------------------------------------------- */

function NavButton({
  active,
  onClick,
  children,
}: {
  active: boolean;
  onClick: () => void;
  children: React.ReactNode;
}) {
  return (
    <button
      onClick={onClick}
      className={`flex items-center gap-2 rounded-xl px-3 py-2 text-sm font-semibold transition ${
        active
          ? "bg-indigo-50 text-indigo-700"
          : "text-zinc-600 hover:bg-zinc-100"
      }`}
    >
      {children}
    </button>
  );
}

/* -------------------------------------------------------
   HOME
------------------------------------------------------- */

function HomeScreen({
  user,
  discoverUsers,
  connectedIds,
  onDiscover,
  onViewUser,
  onConnect,
}: {
  user: User;
  discoverUsers: User[];
  connectedIds: string[];
  onDiscover: () => void;
  onViewUser: (user: User) => void;
  onConnect: (id: string) => void;
}) {
  const topUsers = discoverUsers.slice(0, 3);

  return (
    <div className="space-y-8">

      <section className="rounded-3xl bg-gradient-to-r from-indigo-600 to-purple-600 p-6 text-white shadow-lg md:p-10">
        <p className="text-sm font-semibold text-indigo-100">
          Welcome to InCommon
        </p>

        <h1 className="mt-2 text-3xl font-black md:text-5xl">
          Find your people.
        </h1>

        <p className="mt-4 max-w-2xl text-indigo-100 md:text-lg">
          Discover people and communities based on the things you already
          enjoy — and discover new things along the way.
        </p>

        <button
          onClick={onDiscover}
          className="mt-6 rounded-2xl bg-white px-5 py-3 font-bold text-indigo-700 shadow hover:bg-indigo-50"
        >
          Discover Common Ground
        </button>
      </section>

      <section>
        <div className="mb-4 flex items-end justify-between">
          <div>
            <p className="text-sm font-semibold uppercase tracking-wide text-indigo-600">
              Your Common Ground
            </p>

            <h2 className="mt-1 text-2xl font-black">
              People you may connect with
            </h2>
          </div>

          <button
            onClick={onDiscover}
            className="text-sm font-bold text-indigo-600"
          >
            See all →
          </button>
        </div>

        <div className="grid gap-4 lg:grid-cols-3">
          {topUsers.map((person) => (
            <UserCard
              key={person.id}
              user={person}
              currentUser={user}
              connected={connectedIds.includes(person.id)}
              onView={() => onViewUser(person)}
              onConnect={() => onConnect(person.id)}
            />
          ))}
        </div>
      </section>

      <section className="rounded-3xl border border-zinc-200 bg-white p-6">
        <p className="text-sm font-semibold uppercase tracking-wide text-indigo-600">
          Your interests
        </p>

        <h2 className="mt-1 text-2xl font-black">
          {user.interests.length} things make up your Common Ground
        </h2>

        <div className="mt-5 flex flex-wrap gap-2">
          {user.interests.map((interest) => (
            <Pill key={interest}>{interest}</Pill>
          ))}
        </div>
      </section>
    </div>
  );
}

/* -------------------------------------------------------
   DISCOVER
------------------------------------------------------- */

function DiscoverScreen({
  user,
  users,
  connectedIds,
  onViewUser,
  onConnect,
}: {
  user: User;
  users: User[];
  connectedIds: string[];
  onViewUser: (user: User) => void;
  onConnect: (id: string) => void;
}) {
  const [search, setSearch] = useState("");

  const filtered = users.filter((person) => {
    const value = search.toLowerCase();

    return (
      person.name.toLowerCase().includes(value) ||
      person.interests.some((interest) =>
        interest.toLowerCase().includes(value)
      )
    );
  });

  return (
    <div className="space-y-6">

      <div>
        <p className="text-sm font-semibold uppercase tracking-wide text-indigo-600">
          Discover
        </p>

        <h1 className="mt-1 text-3xl font-black">
          Find people with something in common.
        </h1>

        <p className="mt-2 max-w-2xl text-zinc-600">
          Search people by name or interest. InCommon automatically calculates
          the interests you share.
        </p>
      </div>

      <input
        value={search}
        onChange={(event) => setSearch(event.target.value)}
        placeholder="Search people or interests..."
        className="w-full rounded-2xl border border-zinc-200 bg-white px-5 py-4 outline-none focus:border-indigo-500"
      />

      <div className="grid gap-4 lg:grid-cols-2">
        {filtered.map((person) => (
          <UserCard
            key={person.id}
            user={person}
            currentUser={user}
            connected={connectedIds.includes(person.id)}
            onView={() => onViewUser(person)}
            onConnect={() => onConnect(person.id)}
          />
        ))}
      </div>

      {filtered.length === 0 && (
        <div className="rounded-2xl bg-white p-10 text-center text-zinc-500">
          No people found.
        </div>
      )}
    </div>
  );
}

/* -------------------------------------------------------
   PERSON PROFILE
------------------------------------------------------- */

function PersonProfile({
  user,
  currentUser,
  connected,
  onBack,
  onConnect,
  onMessage,
}: {
  user: User;
  currentUser: User;
  connected: boolean;
  onBack: () => void;
  onConnect: () => void;
  onMessage: () => void;
}) {
  const shared = getSharedInterests(currentUser, user);
  const score = getCommonScore(currentUser, user);

  return (
    <div className="mx-auto max-w-4xl">

      <button
        onClick={onBack}
        className="mb-6 text-sm font-bold text-indigo-600"
      >
        ← Back
      </button>

      <div className="overflow-hidden rounded-3xl border border-zinc-200 bg-white shadow-sm">

        <div className="h-32 bg-gradient-to-r from-indigo-500 to-purple-500" />

        <div className="-mt-12 px-6 pb-8 md:px-10">
          <div className="flex flex-col gap-4 sm:flex-row sm:items-end">
            <div className="flex h-24 w-24 items-center justify-center rounded-full border-4 border-white bg-indigo-100 text-4xl shadow">
              {user.avatar}
            </div>

            <div className="flex-1">
              <h1 className="text-3xl font-black">{user.name}</h1>
              <p className="text-zinc-500">
                {user.username} · {user.location}
              </p>
            </div>

            <div className="flex gap-2">
              {!connected && (
                <button
                  onClick={onConnect}
                  className="rounded-xl bg-indigo-600 px-5 py-3 font-bold text-white"
                >
                  Connect
                </button>
              )}

              <button
                onClick={onMessage}
                className="rounded-xl border border-zinc-200 px-5 py-3 font-bold"
              >
                Message
              </button>
            </div>
          </div>

          <p className="mt-6 text-zinc-600">{user.bio}</p>

          <div className="mt-8 rounded-3xl bg-indigo-50 p-6">
            <p className="text-sm font-bold uppercase tracking-wide text-indigo-600">
              Common Ground
            </p>

            <div className="mt-2 flex items-center gap-3">
              <span className="text-4xl font-black text-indigo-700">
                {score}%
              </span>

              <span className="text-sm text-indigo-900">
                of your interests overlap.
              </span>
            </div>

            <div className="mt-5 flex flex-wrap gap-2">
              {shared.map((interest) => (
                <span
                  key={interest}
                  className="rounded-full bg-white px-3 py-2 text-sm font-semibold text-indigo-700 shadow-sm"
                >
                  ✓ {interest}
                </span>
              ))}
            </div>

            {shared.length > 0 && (
              <p className="mt-5 text-sm text-indigo-900">
                You already have {shared.length} conversation starters.
              </p>
            )}
          </div>

          <div className="mt-8">
            <h2 className="text-xl font-black">Interests</h2>

            <div className="mt-4 flex flex-wrap gap-2">
              {user.interests.map((interest) => (
                <Pill key={interest}>{interest}</Pill>
              ))}
            </div>
          </div>
        </div>
      </div>
    </div>
  );
}

/* -------------------------------------------------------
   COMMUNITIES
------------------------------------------------------- */

function CommunitiesScreen({
  communities,
  joinedCommunities,
  onToggleJoin,
}: {
  communities: Community[];
  joinedCommunities: string[];
  onToggleJoin: (id: string) => void;
}) {
  return (
    <div className="space-y-6">

      <div>
        <p className="text-sm font-semibold uppercase tracking-wide text-indigo-600">
          Communities
        </p>

        <h1 className="mt-1 text-3xl font-black">
          Find people around your interests.
        </h1>
      </div>

      <div className="grid gap-5 md:grid-cols-2">
        {communities.map((community) => {
          const joined = joinedCommunities.includes(community.id);

          return (
            <div
              key={community.id}
              className="rounded-3xl border border-zinc-200 bg-white p-6 shadow-sm"
            >
              <div className="flex items-start justify-between gap-4">
                <div>
                  <p className="text-xs font-bold uppercase tracking-wide text-indigo-600">
                    {community.category}
                  </p>

                  <h2 className="mt-1 text-xl font-black">
                    {community.name}
                  </h2>
                </div>

                <div className="rounded-full bg-zinc-100 px-3 py-1 text-xs font-semibold">
                  {community.members} members
                </div>
              </div>

              <p className="mt-4 text-sm text-zinc-600">
                {community.description}
              </p>

              <div className="mt-4 flex flex-wrap gap-2">
                {community.interests.map((interest) => (
                  <Pill key={interest}>{interest}</Pill>
                ))}
              </div>

              <button
                onClick={() => onToggleJoin(community.id)}
                className={`mt-6 w-full rounded-xl py-3 font-bold ${
                  joined
                    ? "bg-zinc-100 text-zinc-700"
                    : "bg-indigo-600 text-white"
                }`}
              >
                {joined ? "Joined ✓" : "Join Community"}
              </button>
            </div>
          );
        })}
      </div>
    </div>
  );
}

/* -------------------------------------------------------
   CONNECTIONS
------------------------------------------------------- */

function ConnectionsScreen({
  users,
  onViewUser,
  onMessage,
}: {
  users: User[];
  onViewUser: (user: User) => void;
  onMessage: (user: User) => void;
}) {
  return (
    <div className="space-y-6">

      <div>
        <p className="text-sm font-semibold uppercase tracking-wide text-indigo-600">
          Connections
        </p>

        <h1 className="mt-1 text-3xl font-black">
          Your people.
        </h1>
      </div>

      {users.length === 0 ? (
        <div className="rounded-3xl border border-zinc-200 bg-white p-10 text-center">
          <div className="text-4xl">♧</div>

          <h2 className="mt-4 text-xl font-black">
            Your connections will appear here.
          </h2>

          <p className="mt-2 text-sm text-zinc-500">
            Head to Discover to find people who share your interests.
          </p>
        </div>
      ) : (
        <div className="grid gap-4 md:grid-cols-2">
          {users.map((person) => (
            <div
              key={person.id}
              className="rounded-2xl border border-zinc-200 bg-white p-5"
            >
              <div className="flex items-center gap-4">
                <div className="flex h-14 w-14 items-center justify-center rounded-full bg-indigo-100 text-2xl">
                  {person.avatar}
                </div>

                <div className="flex-1">
                  <h3 className="font-bold">{person.name}</h3>
                  <p className="text-sm text-zinc-500">
                    {person.username}
                  </p>
                </div>
              </div>

              <div className="mt-4 flex gap-2">
                <button
                  onClick={() => onViewUser(person)}
                  className="flex-1 rounded-xl border border-zinc-200 py-2 font-semibold"
                >
                  Profile
                </button>

                <button
                  onClick={() => onMessage(person)}
                  className="flex-1 rounded-xl bg-indigo-600 py-2 font-semibold text-white"
                >
                  Message
                </button>
              </div>
            </div>
          ))}
        </div>
      )}
    </div>
  );
}

/* -------------------------------------------------------
   MESSAGES
------------------------------------------------------- */

function MessagesScreen({
  users,
  selectedUser,
  messages,
  messageText,
  setMessageText,
  onSelectUser,
  onSend,
}: {
  users: User[];
  selectedUser: User | null;
  messages: Record<string, { from: string; text: string }[]>;
  messageText: string;
  setMessageText: (value: string) => void;
  onSelectUser: (user: User) => void;
  onSend: () => void;
}) {
  return (
    <div className="overflow-hidden rounded-3xl border border-zinc-200 bg-white">

      <div className="grid min-h-[600px] md:grid-cols-[280px_1fr]">

        <div className="border-b border-zinc-200 md:border-b-0 md:border-r">
          <div className="border-b p-5">
            <h1 className="text-xl font-black">Messages</h1>
          </div>

          {users.length === 0 ? (
            <p className="p-5 text-sm text-zinc-500">
              Connect with someone to start messaging.
            </p>
          ) : (
            users.map((person) => (
              <button
                key={person.id}
                onClick={() => onSelectUser(person)}
                className={`flex w-full items-center gap-3 border-b border-zinc-100 p-4 text-left hover:bg-zinc-50 ${
                  selectedUser?.id === person.id
                    ? "bg-indigo-50"
                    : ""
                }`}
              >
                <div className="flex h-10 w-10 items-center justify-center rounded-full bg-indigo-100">
                  {person.avatar}
                </div>

                <div>
                  <div className="font-bold">{person.name}</div>
                  <div className="text-xs text-zinc-500">
                    {person.interests.slice(0, 2).join(" · ")}
                  </div>
                </div>
              </button>
            ))
          )}
        </div>

        <div className="flex flex-col">

          {!selectedUser ? (
            <div className="flex flex-1 items-center justify-center p-10 text-center">
              <div>
                <div className="text-5xl">✉</div>
                <h2 className="mt-4 text-xl font-black">
                  Start a conversation.
                </h2>
                <p className="mt-2 text-sm text-zinc-500">
                  Choose a connection from the left.
                </p>
              </div>
            </div>
          ) : (
            <>
              <div className="border-b p-5">
                <div className="flex items-center gap-3">
                  <div className="flex h-11 w-11 items-center justify-center rounded-full bg-indigo-100">
                    {selectedUser.avatar}
                  </div>

                  <div>
                    <h2 className="font-black">{selectedUser.name}</h2>
                    <p className="text-xs text-zinc-500">
                      Common Ground:
                      {" "}
                      {getSharedInterests(
                        INITIAL_USER,
                        selectedUser
                      )
                        .slice(0, 3)
                        .join(" · ")}
                    </p>
                  </div>
                </div>
              </div>

              <div className="flex-1 space-y-3 overflow-y-auto p-5">
                {(messages[selectedUser.id] || []).map(
                  (message, index) => (
                    <div
                      key={index}
                      className={`flex ${
                        message.from === "me"
                          ? "justify-end"
                          : "justify-start"
                      }`}
                    >
                      <div
                        className={`max-w-[75%] rounded-2xl px-4 py-3 text-sm ${
                          message.from === "me"
                            ? "bg-indigo-600 text-white"
                            : "bg-zinc-100 text-zinc-800"
                        }`}
                      >
                        {message.text}
                      </div>
                    </div>
                  )
                )}

                {(!messages[selectedUser.id] ||
                  messages[selectedUser.id].length === 0) && (
                  <div className="rounded-2xl bg-indigo-50 p-5 text-sm text-indigo-900">
                    <strong>You have something in common!</strong>
                    <br />
                    Try asking about one of your shared interests.
                  </div>
                )}
              </div>

              <div className="border-t p-4">
                <div className="flex gap-2">
                  <input
                    value={messageText}
                    onChange={(event) =>
                      setMessageText(event.target.value)
                    }
                    onKeyDown={(event) => {
                      if (event.key === "Enter") onSend();
                    }}
                    placeholder="Start a conversation..."
                    className="flex-1 rounded-xl border border-zinc-200 px-4 py-3 outline-none focus:border-indigo-500"
                  />

                  <button
                    onClick={onSend}
                    className="rounded-xl bg-indigo-600 px-5 font-bold text-white"
                  >
                    Send
                  </button>
                </div>
              </div>
            </>
          )}
        </div>
      </div>
    </div>
  );
}

/* -------------------------------------------------------
   PROFILE
------------------------------------------------------- */

function ProfileScreen({
  user,
  profileName,
  profileBio,
  setProfileName,
  setProfileBio,
  interests,
  onToggleInterest,
  onSave,
}: {
  user: User;
  profileName: string;
  profileBio: string;
  setProfileName: (value: string) => void;
  setProfileBio: (value: string) => void;
  interests: string[];
  onToggleInterest: (interest: string) => void;
  onSave: () => void;
}) {
  const [showInterests, setShowInterests] = useState(false);

  return (
    <div className="mx-auto max-w-4xl space-y-6">

      <div>
        <p className="text-sm font-semibold uppercase tracking-wide text-indigo-600">
          Profile
        </p>

        <h1 className="mt-1 text-3xl font-black">
          Your InCommon profile
        </h1>
      </div>

      <div className="rounded-3xl border border-zinc-200 bg-white p-6 shadow-sm md:p-8">

        <div className="flex flex-col gap-5 sm:flex-row sm:items-center">
          <div className="flex h-24 w-24 items-center justify-center rounded-full bg-indigo-100 text-4xl">
            {user.avatar}
          </div>

          <div>
            <h2 className="text-2xl font-black">{user.name}</h2>
            <p className="text-zinc-500">{user.username}</p>
          </div>
        </div>

        <div className="mt-8 space-y-5">

          <div>
            <label className="text-sm font-bold">Name</label>

            <input
              value={profileName}
              onChange={(event) =>
                setProfileName(event.target.value)
              }
              className="mt-2 w-full rounded-xl border border-zinc-200 px-4 py-3"
            />
          </div>

          <div>
            <label className="text-sm font-bold">Bio</label>

            <textarea
              value={profileBio}
              onChange={(event) =>
                setProfileBio(event.target.value)
              }
              rows={4}
              className="mt-2 w-full resize-none rounded-xl border border-zinc-200 px-4 py-3"
            />
          </div>

          <div>
            <div className="flex items-center justify-between">
              <label className="text-sm font-bold">
                Your interests
              </label>

              <button
                onClick={() => setShowInterests(!showInterests)}
                className="text-sm font-bold text-indigo-600"
              >
                {showInterests ? "Hide" : "Edit"}
              </button>
            </div>

            <div className="mt-3 flex flex-wrap gap-2">
              {interests.map((interest) => (
                <Pill key={interest}>{interest}</Pill>
              ))}
            </div>

            {showInterests && (
              <div className="mt-4 max-h-64 overflow-y-auto rounded-2xl border border-zinc-200 p-4">
                <div className="flex flex-wrap gap-2">
                  {ALL_INTERESTS.map((interest) => (
                    <Pill
                      key={interest}
                      selected={interests.includes(interest)}
                      onClick={() => onToggleInterest(interest)}
                    >
                      {interest}
                    </Pill>
                  ))}
                </div>
              </div>
            )}
          </div>

          <button
            onClick={onSave}
            className="w-full rounded-xl bg-indigo-600 py-3 font-bold text-white"
          >
            Save Profile
          </button>
        </div>
      </div>
    </div>
  );
}
