EXE = treefun
TEST = test

MAIN_FILE = main.cpp
TEST_FILE = test.cpp

OBJS_DIR = .objs

OBJS_STUDENT = main.o
OBJS_PROVIDED = coloredout.o

# Discover files for testing...
TEST_CPP += $(filter-out $(MAIN_FILE),$(CPP_FILES))
TEST_CPP += $(TEST_FILE)

TEST_OBJ = $(TEST_CPP:.cpp=.o)

CXX = clang++
LD = clang++
WARNINGS = -pedantic -Wall -Werror -Wfatal-errors -Wextra -Wno-unused-parameter -Wno-unused-variable
CXXFLAGS = -std=c++1y -stdlib=libc++ -g -O0 $(WARNINGS) -MMD -MP -c
LDFLAGS = -std=c++1y -stdlib=libc++ -lc++abi -lpthread

all: $(EXE) $(TEST)

# Pattern rules for object files
$(OBJS_DIR)/%.o: %.cpp | $(OBJS_DIR)
	$(CXX) $(CXXFLAGS) $< -o $@

# Create directories
$(OBJS_DIR):
	@mkdir -p $(OBJS_DIR)

# Rules for executables... unfortunately, we can't use a pattern for the normal executables
$(EXE):
	$(LD) $^ $(LDFLAGS) -o $@

test: $(TEST_OBJ)
	$(LD) $(TEST_OBJ) $(LDFLAGS) $(COVFLAGS) -o $(TEST)

# Executable dependencies
$(EXE):      $(patsubst %.o, $(OBJS_DIR)/%.o,      $(OBJS_STUDENT)) $(patsubst %.o, $(OBJS_DIR)/%.o, $(OBJS_PROVIDED))

# Include automatically generated dependencies
-include $(OBJS_DIR)/*.d

clean:
	rm -rf $(EXE) $(OBJS_DIR) $(TEST) $(TEST_OBJ)

tidy: clean
	rm -rf doc

.PHONY: all tidy clean
